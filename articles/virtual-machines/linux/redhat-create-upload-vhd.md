---
title: "Vytvoření a odeslání Red Hat Enterprise Linux virtuální pevný disk pro použití v Azure | Microsoft Docs"
description: "Naučte se vytvořit a odeslat Azure virtuální pevný disk (VHD) obsahující operační systém Red Hat Linux."
services: virtual-machines-linux
documentationcenter: 
author: szarkos
manager: timlt
editor: tysonn
tags: azure-resource-manager,azure-service-management
ms.assetid: 6c6b8f72-32d3-47fa-be94-6cb54537c69f
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 04/28/2017
ms.author: szark
ms.openlocfilehash: b753c76b8c3d789c681d7fbff6aa07590b860be5
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="prepare-a-red-hat-based-virtual-machine-for-azure"></a><span data-ttu-id="094e0-103">Příprava virtuálního počítače založeného na Red Hat pro Azure</span><span class="sxs-lookup"><span data-stu-id="094e0-103">Prepare a Red Hat-based virtual machine for Azure</span></span>
<span data-ttu-id="094e0-104">V tomto článku se dozvíte, jak připravit virtuální počítač Red Hat Enterprise Linux (RHEL) pro použití v Azure.</span><span class="sxs-lookup"><span data-stu-id="094e0-104">In this article, you will learn how to prepare a Red Hat Enterprise Linux (RHEL) virtual machine for use in Azure.</span></span> <span data-ttu-id="094e0-105">Verze RHEL, které jsou popsané v tomto článku jsou 6.7 + a 7.1 +.</span><span class="sxs-lookup"><span data-stu-id="094e0-105">The versions of RHEL that are covered in this article are 6.7+ and 7.1+.</span></span> <span data-ttu-id="094e0-106">Hypervisory pro přípravu, které jsou popsané v tomto článku jsou virtuální počítače Hyper-V, na základě jádra (KVM) a VMware.</span><span class="sxs-lookup"><span data-stu-id="094e0-106">The hypervisors for preparation that are covered in this article are Hyper-V, kernel-based virtual machine (KVM), and VMware.</span></span> <span data-ttu-id="094e0-107">Další informace o požadavcích na podmínky pro účasti v programu Red Hat přístup ke cloudu najdete v tématu [webu přístup do cloudu Red Hat](http://www.redhat.com/en/technologies/cloud-computing/cloud-access) a [systémem RHEL v Azure](https://access.redhat.com/ecosystem/ccsp/microsoft-azure).</span><span class="sxs-lookup"><span data-stu-id="094e0-107">For more information about eligibility requirements for participating in Red Hat's Cloud Access program, see [Red Hat's Cloud Access website](http://www.redhat.com/en/technologies/cloud-computing/cloud-access) and [Running RHEL on Azure](https://access.redhat.com/ecosystem/ccsp/microsoft-azure).</span></span>

## <a name="prepare-a-red-hat-based-virtual-machine-from-hyper-v-manager"></a><span data-ttu-id="094e0-108">Příprava virtuálního počítače, na základě Red Hat ze Správce technologie Hyper-V</span><span class="sxs-lookup"><span data-stu-id="094e0-108">Prepare a Red Hat-based virtual machine from Hyper-V Manager</span></span>

### <a name="prerequisites"></a><span data-ttu-id="094e0-109">Požadavky</span><span class="sxs-lookup"><span data-stu-id="094e0-109">Prerequisites</span></span>
<span data-ttu-id="094e0-110">V této části předpokládá, že jste již získat soubor ISO z webu Red Hat a nainstalovat bitovou kopii systému RHEL na virtuální pevný disk (VHD).</span><span class="sxs-lookup"><span data-stu-id="094e0-110">This section assumes that you have already obtained an ISO file from the Red Hat website and installed the RHEL image to a virtual hard disk (VHD).</span></span> <span data-ttu-id="094e0-111">Další podrobnosti o tom, jak nainstalovat bitovou kopii operačního systému pomocí Správce technologie Hyper-V najdete v tématu [nainstalovat roli Hyper-V a konfigurace virtuálního počítače](http://technet.microsoft.com/library/hh846766.aspx).</span><span class="sxs-lookup"><span data-stu-id="094e0-111">For more details about how to use Hyper-V Manager to install an operating system image, see [Install the Hyper-V Role and Configure a Virtual Machine](http://technet.microsoft.com/library/hh846766.aspx).</span></span>

<span data-ttu-id="094e0-112">**Poznámky k instalaci RHEL**</span><span class="sxs-lookup"><span data-stu-id="094e0-112">**RHEL installation notes**</span></span>

* <span data-ttu-id="094e0-113">Azure nepodporuje formátu VHDX.</span><span class="sxs-lookup"><span data-stu-id="094e0-113">Azure does not support the VHDX format.</span></span> <span data-ttu-id="094e0-114">Azure podporuje pouze dlouhodobý virtuálního pevného disku.</span><span class="sxs-lookup"><span data-stu-id="094e0-114">Azure supports only fixed VHD.</span></span> <span data-ttu-id="094e0-115">Převést disk na formát virtuálního pevného disku můžete použít Správce technologie Hyper-V, nebo můžete pomocí rutiny convert-VHD prostředí.</span><span class="sxs-lookup"><span data-stu-id="094e0-115">You can use Hyper-V Manager to convert the disk to VHD format, or you can use the convert-vhd cmdlet.</span></span> <span data-ttu-id="094e0-116">Pokud používáte VirtualBox, vyberte **pevnou velikost** oproti výchozí možnost přidělí dynamicky při vytváření disku.</span><span class="sxs-lookup"><span data-stu-id="094e0-116">If you use VirtualBox, select **Fixed size** as opposed to the default dynamically allocated option when you create the disk.</span></span>
* <span data-ttu-id="094e0-117">Azure podporuje pouze virtuální počítače generace 1.</span><span class="sxs-lookup"><span data-stu-id="094e0-117">Azure supports only generation 1 virtual machines.</span></span> <span data-ttu-id="094e0-118">Virtuální počítač generace 1 můžete převést z VHDX do formátu souboru virtuálního pevného disku a dynamicky se zvětšující na disk pevné velikosti.</span><span class="sxs-lookup"><span data-stu-id="094e0-118">You can convert a generation 1 virtual machine from VHDX to the VHD file format and from dynamically expanding to a fixed-size disk.</span></span> <span data-ttu-id="094e0-119">Nelze změnit generaci virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="094e0-119">You can't change a virtual machine's generation.</span></span> <span data-ttu-id="094e0-120">Další informace najdete v tématu [by měl vytvořit virtuální počítač generace 1 nebo 2 v Hyper-V?](https://technet.microsoft.com/windows-server-docs/compute/hyper-v/plan/should-i-create-a-generation-1-or-2-virtual-machine-in-hyper-v).</span><span class="sxs-lookup"><span data-stu-id="094e0-120">For more information, see [Should I create a generation 1 or 2 virtual machine in Hyper-V?](https://technet.microsoft.com/windows-server-docs/compute/hyper-v/plan/should-i-create-a-generation-1-or-2-virtual-machine-in-hyper-v).</span></span>
* <span data-ttu-id="094e0-121">Maximální velikost povolenou pro virtuální pevný disk je 1,023 GB.</span><span class="sxs-lookup"><span data-stu-id="094e0-121">The maximum size that's allowed for the VHD is 1,023 GB.</span></span>
* <span data-ttu-id="094e0-122">Při instalaci operačního systému Linux, doporučujeme použít standardní oddíly spíše než logické svazku Manager (LVM), což je často na výchozí hodnoty pro mnoho instalace.</span><span class="sxs-lookup"><span data-stu-id="094e0-122">When you install the Linux operating system, we recommend that you use standard partitions rather than Logical Volume Manager (LVM), which is often the default for many installations.</span></span> <span data-ttu-id="094e0-123">Tento postup zabráníte LVM název je v konfliktu s klonovaný virtuální počítače, zvlášť pokud byste někdy potřebovali pro připojení k jiné identický virtuální počítač pro řešení potíží s diskem operačního systému.</span><span class="sxs-lookup"><span data-stu-id="094e0-123">This practice will avoid LVM name conflicts with cloned virtual machines, particularly if you ever need to attach an operating system disk to another identical virtual machine for troubleshooting.</span></span> <span data-ttu-id="094e0-124">[LVM](configure-lvm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) nebo [RAID](configure-raid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) mohou být použity na datové disky.</span><span class="sxs-lookup"><span data-stu-id="094e0-124">[LVM](configure-lvm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) or [RAID](configure-raid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) may be used on data disks.</span></span>
* <span data-ttu-id="094e0-125">Vyžaduje se podpora jádra pro připojení systémy souborů univerzální formát disku (UDF).</span><span class="sxs-lookup"><span data-stu-id="094e0-125">Kernel support for mounting Universal Disk Format (UDF) file systems is required.</span></span> <span data-ttu-id="094e0-126">Při prvním spuštění v Azure formátu UDF média, který je připojen k Host předá konfiguraci zřizování virtuální počítač Linux.</span><span class="sxs-lookup"><span data-stu-id="094e0-126">At first boot on Azure, the UDF-formatted media that is attached to the guest passes the provisioning configuration to the Linux virtual machine.</span></span> <span data-ttu-id="094e0-127">Azure Linux Agent musí být schopný se připojit a načíst jeho konfiguraci a zřízení virtuálního počítače v systému souborů UDF.</span><span class="sxs-lookup"><span data-stu-id="094e0-127">The Azure Linux Agent must be able to mount the UDF file system to read its configuration and provision the virtual machine.</span></span>
* <span data-ttu-id="094e0-128">Verze jádra systému Linux, které jsou starší než 2.6.37 nepodporují přístup k nerovnoměrné paměti (NUMA) s technologií Hyper-V s větší velikostí virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="094e0-128">Versions of the Linux kernel that are earlier than 2.6.37 do not support non-uniform memory access (NUMA) on Hyper-V with larger virtual machine sizes.</span></span> <span data-ttu-id="094e0-129">-Li tento problém ovlivňuje především starší distribuce, které používají nadřazený Red Hat 2.6.32 jádra která byla opravena v RHEL 6.6 (jádra 2.6.32 504).</span><span class="sxs-lookup"><span data-stu-id="094e0-129">This issue primarily impacts older distributions that use the upstream Red Hat 2.6.32 kernel and was fixed in RHEL 6.6 (kernel-2.6.32-504).</span></span> <span data-ttu-id="094e0-130">Systémy, které spouštějí vlastní jádra, které jsou starší než 2.6.37 nebo na základě RHEL jádra, která jsou starší než 2.6.32-504 musíte nastavit `numa=off` spouštění na příkazovém řádku jádra v grub.conf parametr.</span><span class="sxs-lookup"><span data-stu-id="094e0-130">Systems that run custom kernels that are older than 2.6.37 or RHEL-based kernels that are older than 2.6.32-504 must set the `numa=off` boot parameter on the kernel command line in grub.conf.</span></span> <span data-ttu-id="094e0-131">Další informace najdete v tématu Red Hat [KB 436883](https://access.redhat.com/solutions/436883).</span><span class="sxs-lookup"><span data-stu-id="094e0-131">For more information, see Red Hat [KB 436883](https://access.redhat.com/solutions/436883).</span></span>
* <span data-ttu-id="094e0-132">Nekonfigurujte přepnutí oddílu na disku operačního systému.</span><span class="sxs-lookup"><span data-stu-id="094e0-132">Do not configure a swap partition on the operating system disk.</span></span> <span data-ttu-id="094e0-133">Chcete-li vytvořit odkládací soubor na disku dočasných prostředků lze nakonfigurovat agenta systému Linux.</span><span class="sxs-lookup"><span data-stu-id="094e0-133">The Linux Agent can be configured to create a swap file on the temporary resource disk.</span></span>  <span data-ttu-id="094e0-134">Další informace o této naleznete v následujících krocích.</span><span class="sxs-lookup"><span data-stu-id="094e0-134">More information about this can be found in the following steps.</span></span>
* <span data-ttu-id="094e0-135">Všechny virtuální pevné disky musí mít velikostí, které jsou násobky 1 MB.</span><span class="sxs-lookup"><span data-stu-id="094e0-135">All VHDs must have sizes that are multiples of 1 MB.</span></span>

### <a name="prepare-a-rhel-6-virtual-machine-from-hyper-v-manager"></a><span data-ttu-id="094e0-136">Příprava systému RHEL 6 virtuálního počítače ze Správce technologie Hyper-V</span><span class="sxs-lookup"><span data-stu-id="094e0-136">Prepare a RHEL 6 virtual machine from Hyper-V Manager</span></span>

1. <span data-ttu-id="094e0-137">Ve Správci technologie Hyper-V vyberte virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="094e0-137">In Hyper-V Manager, select the virtual machine.</span></span>

2. <span data-ttu-id="094e0-138">Klikněte na tlačítko **Connect** k otevření okna konzoly pro virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="094e0-138">Click **Connect** to open a console window for the virtual machine.</span></span>

3. <span data-ttu-id="094e0-139">V systému RHEL 6 NetworkManager může narušovat Azure Linux agent.</span><span class="sxs-lookup"><span data-stu-id="094e0-139">In RHEL 6, NetworkManager can interfere with the Azure Linux agent.</span></span> <span data-ttu-id="094e0-140">Odinstalaci tohoto balíčku tak, že spustíte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="094e0-140">Uninstall this package by running the following command:</span></span>
   
        # sudo rpm -e --nodeps NetworkManager

4. <span data-ttu-id="094e0-141">Vytvořit nebo upravit `/etc/sysconfig/network` souboru a přidejte následující text:</span><span class="sxs-lookup"><span data-stu-id="094e0-141">Create or edit the `/etc/sysconfig/network` file, and add the following text:</span></span>
   
        NETWORKING=yes
        HOSTNAME=localhost.localdomain

5. <span data-ttu-id="094e0-142">Vytvořit nebo upravit `/etc/sysconfig/network-scripts/ifcfg-eth0` souboru a přidejte následující text:</span><span class="sxs-lookup"><span data-stu-id="094e0-142">Create or edit the `/etc/sysconfig/network-scripts/ifcfg-eth0` file, and add the following text:</span></span>
   
        DEVICE=eth0
        ONBOOT=yes
        BOOTPROTO=dhcp
        TYPE=Ethernet
        USERCTL=no
        PEERDNS=yes
        IPV6INIT=no

6. <span data-ttu-id="094e0-143">(Nebo odebrat) udev pravidel tak, aby nedošlo ke generování statická pravidla pro rozhraní sítě Ethernet.</span><span class="sxs-lookup"><span data-stu-id="094e0-143">Move (or remove) the udev rules to avoid generating static rules for the Ethernet interface.</span></span> <span data-ttu-id="094e0-144">Tato pravidla způsobit problémy při klonování virtuálního počítače v Microsoft Azure nebo Hyper-V:</span><span class="sxs-lookup"><span data-stu-id="094e0-144">These rules cause problems when you clone a virtual machine in Microsoft Azure or Hyper-V:</span></span>

        # sudo ln -s /dev/null /etc/udev/rules.d/75-persistent-net-generator.rules
        
        # sudo rm -f /etc/udev/rules.d/70-persistent-net.rules

7. <span data-ttu-id="094e0-145">Ujistěte se, že síťové služby se spustí při spuštění spuštěním následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="094e0-145">Ensure that the network service will start at boot time by running the following command:</span></span>

        # sudo chkconfig network on

8. <span data-ttu-id="094e0-146">Registrujte předplatné Red Hat povolit instalaci balíčků z úložiště RHEL spuštěním následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="094e0-146">Register your Red Hat subscription to enable the installation of packages from the RHEL repository by running the following command:</span></span>

        # sudo subscription-manager register --auto-attach --username=XXX --password=XXX

9. <span data-ttu-id="094e0-147">Balíček WALinuxAgent `WALinuxAgent-<version>`, bylo posunuto do Red Hat funkce úložiště.</span><span class="sxs-lookup"><span data-stu-id="094e0-147">The WALinuxAgent package, `WALinuxAgent-<version>`, has been pushed to the Red Hat extras repository.</span></span> <span data-ttu-id="094e0-148">Povolení funkce úložiště tak, že spustíte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="094e0-148">Enable the extras repository by running the following command:</span></span>

        # subscription-manager repos --enable=rhel-6-server-extras-rpms

10. <span data-ttu-id="094e0-149">Upravte řádku spouštěcí jádra ve vaší konfiguraci grub pro zahrnutí dalších jádra parametry Azure.</span><span class="sxs-lookup"><span data-stu-id="094e0-149">Modify the kernel boot line in your grub configuration to include additional kernel parameters for Azure.</span></span> <span data-ttu-id="094e0-150">Chcete-li provést tuto změny, otevřete `/boot/grub/menu.lst` v textovém editoru a ujistěte se, že výchozí jádra zahrnuje následující parametry:</span><span class="sxs-lookup"><span data-stu-id="094e0-150">To do this modification, open `/boot/grub/menu.lst` in a text editor, and ensure that the default kernel includes the following parameters:</span></span>
    
        console=ttyS0 earlyprintk=ttyS0 rootdelay=300
    
    <span data-ttu-id="094e0-151">To také zajistí, že všechny zprávy konzoly odešlou do první sériového portu, který může být užitečné Azure podporu pro ladění problémů.</span><span class="sxs-lookup"><span data-stu-id="094e0-151">This will also ensure that all console messages are sent to the first serial port, which can assist Azure support with debugging issues.</span></span>
    
    <span data-ttu-id="094e0-152">Kromě toho je vhodné odebrat tyto parametry:</span><span class="sxs-lookup"><span data-stu-id="094e0-152">In addition, we recommended that you remove the following parameters:</span></span>
    
        rhgb quiet crashkernel=auto
    
    <span data-ttu-id="094e0-153">Grafické a quiet spouštěcí nejsou užitečné při cloudovém prostředí, kde chceme, aby všechny protokoly pro odeslání do sériového portu.</span><span class="sxs-lookup"><span data-stu-id="094e0-153">Graphical and quiet boot are not useful in a cloud environment where we want all the logs to be sent to the serial port.</span></span>  <span data-ttu-id="094e0-154">Můžete nechat `crashkernel` možnost nakonfigurované v případě potřeby.</span><span class="sxs-lookup"><span data-stu-id="094e0-154">You can leave the `crashkernel` option configured if desired.</span></span> <span data-ttu-id="094e0-155">Všimněte si, že tento parametr snižuje množství dostupné paměti ve virtuálním počítači 128 MB a víc.</span><span class="sxs-lookup"><span data-stu-id="094e0-155">Note that this parameter reduces the amount of available memory in the virtual machine by 128 MB or more.</span></span> <span data-ttu-id="094e0-156">Tato konfigurace může způsobovat problémy menší velikostí virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="094e0-156">This configuration might be problematic on smaller virtual machine sizes.</span></span>

    >[!Important]
    <span data-ttu-id="094e0-157">RHEL verze 6.5 a starší, musíte taky nastavit `numa=off` jádra parametr.</span><span class="sxs-lookup"><span data-stu-id="094e0-157">RHEL 6.5 and earlier must also set the `numa=off` kernel parameter.</span></span> <span data-ttu-id="094e0-158">V tématu Red Hat [KB 436883](https://access.redhat.com/solutions/436883).</span><span class="sxs-lookup"><span data-stu-id="094e0-158">See Red Hat [KB 436883](https://access.redhat.com/solutions/436883).</span></span>

11. <span data-ttu-id="094e0-159">Zajistěte, aby zabezpečené shell (SSH) server je nainstalován a nakonfigurován na spuštění při spuštění, což obvykle výchozí nastavení.</span><span class="sxs-lookup"><span data-stu-id="094e0-159">Ensure that the secure shell (SSH) server is installed and configured to start at boot time, which is usually the default.</span></span> <span data-ttu-id="094e0-160">Upravte /etc/ssh/sshd_config obsahovala následující řádek:</span><span class="sxs-lookup"><span data-stu-id="094e0-160">Modify /etc/ssh/sshd_config to include the following line:</span></span>

        ClientAliveInterval 180

12. <span data-ttu-id="094e0-161">Nainstalujte Azure Linux Agent spuštěním následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="094e0-161">Install the Azure Linux Agent by running the following command:</span></span>

        # sudo yum install WALinuxAgent

        # sudo chkconfig waagent on

    <span data-ttu-id="094e0-162">Instalace balíčku WALinuxAgent odebere NetworkManager a balíčky NetworkManager gnome Pokud již nebyly odebrány v kroku 3.</span><span class="sxs-lookup"><span data-stu-id="094e0-162">Installing the WALinuxAgent package removes the NetworkManager and NetworkManager-gnome packages if they were not already removed in step 3.</span></span>

13. <span data-ttu-id="094e0-163">Nevytvářejte odkládacího prostoru na disku operačního systému.</span><span class="sxs-lookup"><span data-stu-id="094e0-163">Do not create swap space on the operating system disk.</span></span>

    <span data-ttu-id="094e0-164">Azure Linux Agent mohou automaticky konfigurovat odkládacího souboru pomocí disku místní prostředek, který je připojen k virtuálnímu počítači po zřízení virtuálního počítače na platformě Azure.</span><span class="sxs-lookup"><span data-stu-id="094e0-164">The Azure Linux Agent can automatically configure swap space by using the local resource disk that is attached to the virtual machine after the virtual machine is provisioned on Azure.</span></span> <span data-ttu-id="094e0-165">Všimněte si, že disk místní prostředek je dočasný a že ji může vyprázdnit když virtuální počítač je zrušit.</span><span class="sxs-lookup"><span data-stu-id="094e0-165">Note that the local resource disk is a temporary disk and that it might be emptied when the virtual machine is deprovisioned.</span></span> <span data-ttu-id="094e0-166">Po instalaci Azure Linux Agent v předchozím kroku, upravte následující parametry v /etc/waagent.conf odpovídajícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="094e0-166">After you install the Azure Linux Agent in the previous step, modify the following parameters in /etc/waagent.conf appropriately:</span></span>

        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this to whatever you need it to be.

14. <span data-ttu-id="094e0-167">Zrušení registrace předplatného (v případě potřeby) tak, že spustíte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="094e0-167">Unregister the subscription (if necessary) by running the following command:</span></span>

        # sudo subscription-manager unregister

15. <span data-ttu-id="094e0-168">Spusťte následující příkaz pro zrušení zřízení virtuálního počítače a jeho přípravu pro zřizování na Azure:</span><span class="sxs-lookup"><span data-stu-id="094e0-168">Run the following commands to deprovision the virtual machine and prepare it for provisioning on Azure:</span></span>

        # sudo waagent -force -deprovision

        # export HISTSIZE=0

        # logout

16. <span data-ttu-id="094e0-169">Klikněte na tlačítko **akce** > **vypnout** ve Správci technologie Hyper-V.</span><span class="sxs-lookup"><span data-stu-id="094e0-169">Click **Action** > **Shut Down** in Hyper-V Manager.</span></span> <span data-ttu-id="094e0-170">Svůj disk VHD Linux je nyní připravena k odeslání do Azure.</span><span class="sxs-lookup"><span data-stu-id="094e0-170">Your Linux VHD is now ready to be uploaded to Azure.</span></span>


### <a name="prepare-a-rhel-7-virtual-machine-from-hyper-v-manager"></a><span data-ttu-id="094e0-171">Příprava RHEL 7 virtuálního počítače ze Správce technologie Hyper-V</span><span class="sxs-lookup"><span data-stu-id="094e0-171">Prepare a RHEL 7 virtual machine from Hyper-V Manager</span></span>

1. <span data-ttu-id="094e0-172">Ve Správci technologie Hyper-V vyberte virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="094e0-172">In Hyper-V Manager, select the virtual machine.</span></span>

2. <span data-ttu-id="094e0-173">Klikněte na tlačítko **Connect** k otevření okna konzoly pro virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="094e0-173">Click **Connect** to open a console window for the virtual machine.</span></span>

3. <span data-ttu-id="094e0-174">Vytvořit nebo upravit `/etc/sysconfig/network` souboru a přidejte následující text:</span><span class="sxs-lookup"><span data-stu-id="094e0-174">Create or edit the `/etc/sysconfig/network` file, and add the following text:</span></span>
   
        NETWORKING=yes
        HOSTNAME=localhost.localdomain

4. <span data-ttu-id="094e0-175">Vytvořit nebo upravit `/etc/sysconfig/network-scripts/ifcfg-eth0` souboru a přidejte následující text:</span><span class="sxs-lookup"><span data-stu-id="094e0-175">Create or edit the `/etc/sysconfig/network-scripts/ifcfg-eth0` file, and add the following text:</span></span>
   
        DEVICE=eth0
        ONBOOT=yes
        BOOTPROTO=dhcp
        TYPE=Ethernet
        USERCTL=no
        PEERDNS=yes
        IPV6INIT=no
        NM_CONTROLLED=no

5. <span data-ttu-id="094e0-176">Ujistěte se, že síťové služby se spustí při spuštění spuštěním následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="094e0-176">Ensure that the network service will start at boot time by running the following command:</span></span>

        # sudo chkconfig network on

6. <span data-ttu-id="094e0-177">Registrujte předplatné Red Hat povolit instalaci balíčků z úložiště RHEL spuštěním následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="094e0-177">Register your Red Hat subscription to enable the installation of packages from the RHEL repository by running the following command:</span></span>

        # sudo subscription-manager register --auto-attach --username=XXX --password=XXX

7. <span data-ttu-id="094e0-178">Upravte řádku spouštěcí jádra ve vaší konfiguraci grub pro zahrnutí dalších jádra parametry Azure.</span><span class="sxs-lookup"><span data-stu-id="094e0-178">Modify the kernel boot line in your grub configuration to include additional kernel parameters for Azure.</span></span> <span data-ttu-id="094e0-179">Chcete-li provést tuto změny, otevřete `/etc/default/grub` v textovém editoru a upravit `GRUB_CMDLINE_LINUX` parametr.</span><span class="sxs-lookup"><span data-stu-id="094e0-179">To do this modification, open `/etc/default/grub` in a text editor, and edit the `GRUB_CMDLINE_LINUX` parameter.</span></span> <span data-ttu-id="094e0-180">Například:</span><span class="sxs-lookup"><span data-stu-id="094e0-180">For example:</span></span>
   
        GRUB_CMDLINE_LINUX="rootdelay=300 console=ttyS0 earlyprintk=ttyS0 net.ifnames=0"
   
   <span data-ttu-id="094e0-181">To také zajistí, že všechny zprávy konzoly odešlou do první sériového portu, který může být užitečné Azure podporu pro ladění problémů.</span><span class="sxs-lookup"><span data-stu-id="094e0-181">This will also ensure that all console messages are sent to the first serial port, which can assist Azure support with debugging issues.</span></span> <span data-ttu-id="094e0-182">Tato konfigurace taky vypne nové zásady vytváření názvů RHEL 7 pro síťové adaptéry.</span><span class="sxs-lookup"><span data-stu-id="094e0-182">This configuration also turns off the new RHEL 7 naming conventions for NICs.</span></span> <span data-ttu-id="094e0-183">Kromě toho doporučujeme odebrat následující parametry:</span><span class="sxs-lookup"><span data-stu-id="094e0-183">In addition, we recommend that you remove the following parameters:</span></span>
   
        rhgb quiet crashkernel=auto
   
    <span data-ttu-id="094e0-184">Grafické a quiet spouštěcí nejsou užitečné při cloudovém prostředí, kde chceme, aby všechny protokoly pro odeslání do sériového portu.</span><span class="sxs-lookup"><span data-stu-id="094e0-184">Graphical and quiet boot are not useful in a cloud environment where we want all the logs to be sent to the serial port.</span></span> <span data-ttu-id="094e0-185">Můžete nechat `crashkernel` možnost nakonfigurované v případě potřeby.</span><span class="sxs-lookup"><span data-stu-id="094e0-185">You can leave the `crashkernel` option configured if desired.</span></span> <span data-ttu-id="094e0-186">Všimněte si, že tento parametr snižuje množství dostupné paměti ve virtuálním počítači 128 MB a víc, což může způsobovat problémy menší velikostí virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="094e0-186">Note that this parameter reduces the amount of available memory in the virtual machine by 128 MB or more, which might be problematic on smaller virtual machine sizes.</span></span>

8. <span data-ttu-id="094e0-187">Po dokončení úprav `/etc/default/grub`, spusťte následující příkaz k opětovnému sestavení grub konfigurace:</span><span class="sxs-lookup"><span data-stu-id="094e0-187">After you are done editing `/etc/default/grub`, run the following command to rebuild the grub configuration:</span></span>

        # sudo grub2-mkconfig -o /boot/grub2/grub.cfg

9. <span data-ttu-id="094e0-188">Zajistěte, aby serverem SSH je nainstalován a nakonfigurován na spuštění při spuštění, což obvykle výchozí nastavení.</span><span class="sxs-lookup"><span data-stu-id="094e0-188">Ensure that the SSH server is installed and configured to start at boot time, which is usually the default.</span></span> <span data-ttu-id="094e0-189">Upravit `/etc/ssh/sshd_config` obsahovala následující řádek:</span><span class="sxs-lookup"><span data-stu-id="094e0-189">Modify `/etc/ssh/sshd_config` to include the following line:</span></span>

        ClientAliveInterval 180

10. <span data-ttu-id="094e0-190">Balíček WALinuxAgent `WALinuxAgent-<version>`, bylo posunuto do Red Hat funkce úložiště.</span><span class="sxs-lookup"><span data-stu-id="094e0-190">The WALinuxAgent package, `WALinuxAgent-<version>`, has been pushed to the Red Hat extras repository.</span></span> <span data-ttu-id="094e0-191">Povolení funkce úložiště tak, že spustíte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="094e0-191">Enable the extras repository by running the following command:</span></span>

        # subscription-manager repos --enable=rhel-7-server-extras-rpms

11. <span data-ttu-id="094e0-192">Nainstalujte Azure Linux Agent spuštěním následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="094e0-192">Install the Azure Linux Agent by running the following command:</span></span>

        # sudo yum install WALinuxAgent

        # sudo systemctl enable waagent.service

12. <span data-ttu-id="094e0-193">Nevytvářejte odkládacího prostoru na disku operačního systému.</span><span class="sxs-lookup"><span data-stu-id="094e0-193">Do not create swap space on the operating system disk.</span></span>

    <span data-ttu-id="094e0-194">Azure Linux Agent mohou automaticky konfigurovat odkládacího souboru pomocí disku místní prostředek, který je připojen k virtuálnímu počítači po zřízení virtuálního počítače na platformě Azure.</span><span class="sxs-lookup"><span data-stu-id="094e0-194">The Azure Linux Agent can automatically configure swap space by using the local resource disk that is attached to the virtual machine after the virtual machine is provisioned on Azure.</span></span> <span data-ttu-id="094e0-195">Upozorňujeme, že disk místní prostředek je dočasný a může být vyprázdněny, když virtuální počítač je zrušit.</span><span class="sxs-lookup"><span data-stu-id="094e0-195">Note that the local resource disk is a temporary disk, and it might be emptied when the virtual machine is deprovisioned.</span></span> <span data-ttu-id="094e0-196">Po instalaci Azure Linux Agent v předchozím kroku, upravte následující parametry v `/etc/waagent.conf` odpovídajícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="094e0-196">After you install the Azure Linux Agent in the previous step, modify the following parameters in `/etc/waagent.conf` appropriately:</span></span>

        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this to whatever you need it to be.

13. <span data-ttu-id="094e0-197">Pokud chcete zrušit registraci předplatného, spusťte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="094e0-197">If you want to unregister the subscription, run the following command:</span></span>

        # sudo subscription-manager unregister

14. <span data-ttu-id="094e0-198">Spusťte následující příkaz pro zrušení zřízení virtuálního počítače a jeho přípravu pro zřizování na Azure:</span><span class="sxs-lookup"><span data-stu-id="094e0-198">Run the following commands to deprovision the virtual machine and prepare it for provisioning on Azure:</span></span>

        # sudo waagent -force -deprovision

        # export HISTSIZE=0

        # logout

15. <span data-ttu-id="094e0-199">Klikněte na tlačítko **akce** > **vypnout** ve Správci technologie Hyper-V.</span><span class="sxs-lookup"><span data-stu-id="094e0-199">Click **Action** > **Shut Down** in Hyper-V Manager.</span></span> <span data-ttu-id="094e0-200">Svůj disk VHD Linux je nyní připravena k odeslání do Azure.</span><span class="sxs-lookup"><span data-stu-id="094e0-200">Your Linux VHD is now ready to be uploaded to Azure.</span></span>


## <a name="prepare-a-red-hat-based-virtual-machine-from-kvm"></a><span data-ttu-id="094e0-201">Příprava virtuálního počítače, na základě Red Hat z KVM</span><span class="sxs-lookup"><span data-stu-id="094e0-201">Prepare a Red Hat-based virtual machine from KVM</span></span>
### <a name="prepare-a-rhel-6-virtual-machine-from-kvm"></a><span data-ttu-id="094e0-202">Příprava systému RHEL 6 virtuálního počítače z KVM</span><span class="sxs-lookup"><span data-stu-id="094e0-202">Prepare a RHEL 6 virtual machine from KVM</span></span>

1. <span data-ttu-id="094e0-203">Stáhněte z webu Red Hat KVM bitové kopie systému RHEL 6.</span><span class="sxs-lookup"><span data-stu-id="094e0-203">Download the KVM image of RHEL 6 from the Red Hat website.</span></span>

2. <span data-ttu-id="094e0-204">Nastavení kořenové heslo.</span><span class="sxs-lookup"><span data-stu-id="094e0-204">Set a root password.</span></span>

    <span data-ttu-id="094e0-205">Generovat zašifrované heslo a zkopírujte výstup tohoto příkazu:</span><span class="sxs-lookup"><span data-stu-id="094e0-205">Generate an encrypted password, and copy the output of the command:</span></span>

        # openssl passwd -1 changeme

    <span data-ttu-id="094e0-206">Nastavení kořenové heslo s guestfish:</span><span class="sxs-lookup"><span data-stu-id="094e0-206">Set a root password with guestfish:</span></span>
        
        # guestfish --rw -a <image-name>
        > <fs> run
        > <fs> list-filesystems
        > <fs> mount /dev/sda1 /
        > <fs> vi /etc/shadow
        > <fs> exit

   <span data-ttu-id="094e0-207">Změňte druhé pole uživatele root z "!"</span><span class="sxs-lookup"><span data-stu-id="094e0-207">Change the second field of the root user from "!!"</span></span> <span data-ttu-id="094e0-208">do zašifrované heslo.</span><span class="sxs-lookup"><span data-stu-id="094e0-208">to the encrypted password.</span></span>

3. <span data-ttu-id="094e0-209">Vytvoření virtuálního počítače v KVM z qcow2 bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="094e0-209">Create a virtual machine in KVM from the qcow2 image.</span></span> <span data-ttu-id="094e0-210">Nastavte typ disku na **qcow2**a nastavte model zařízení rozhraní virtuální sítě na **virtio**.</span><span class="sxs-lookup"><span data-stu-id="094e0-210">Set the disk type to **qcow2**, and set the virtual network interface device model to **virtio**.</span></span> <span data-ttu-id="094e0-211">Potom spusťte virtuální počítač a přihlaste se jako kořenový.</span><span class="sxs-lookup"><span data-stu-id="094e0-211">Then, start the virtual machine, and sign in as root.</span></span>

4. <span data-ttu-id="094e0-212">Vytvořit nebo upravit `/etc/sysconfig/network` souboru a přidejte následující text:</span><span class="sxs-lookup"><span data-stu-id="094e0-212">Create or edit the `/etc/sysconfig/network` file, and add the following text:</span></span>
   
        NETWORKING=yes
        HOSTNAME=localhost.localdomain

5. <span data-ttu-id="094e0-213">Vytvořit nebo upravit `/etc/sysconfig/network-scripts/ifcfg-eth0` souboru a přidejte následující text:</span><span class="sxs-lookup"><span data-stu-id="094e0-213">Create or edit the `/etc/sysconfig/network-scripts/ifcfg-eth0` file, and add the following text:</span></span>
   
        DEVICE=eth0
        ONBOOT=yes
        BOOTPROTO=dhcp
        TYPE=Ethernet
        USERCTL=no
        PEERDNS=yes
        IPV6INIT=no

6. <span data-ttu-id="094e0-214">(Nebo odebrat) udev pravidel tak, aby nedošlo ke generování statická pravidla pro rozhraní sítě Ethernet.</span><span class="sxs-lookup"><span data-stu-id="094e0-214">Move (or remove) the udev rules to avoid generating static rules for the Ethernet interface.</span></span> <span data-ttu-id="094e0-215">Tato pravidla způsobit problémy při klonování virtuálního počítače v Azure nebo Hyper-V:</span><span class="sxs-lookup"><span data-stu-id="094e0-215">These rules cause problems when you clone a virtual machine in Azure or Hyper-V:</span></span>

        # sudo ln -s /dev/null /etc/udev/rules.d/75-persistent-net-generator.rules

        # sudo rm -f /etc/udev/rules.d/70-persistent-net.rules

7. <span data-ttu-id="094e0-216">Ujistěte se, že síťové služby se spustí při spuštění spuštěním následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="094e0-216">Ensure that the network service will start at boot time by running the following command:</span></span>

        # chkconfig network on

8. <span data-ttu-id="094e0-217">Registrujte předplatné Red Hat povolit instalaci balíčků z úložiště RHEL spuštěním následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="094e0-217">Register your Red Hat subscription to enable the installation of packages from the RHEL repository by running the following command:</span></span>

        # subscription-manager register --auto-attach --username=XXX --password=XXX

9. <span data-ttu-id="094e0-218">Upravte řádku spouštěcí jádra ve vaší konfiguraci grub pro zahrnutí dalších jádra parametry Azure.</span><span class="sxs-lookup"><span data-stu-id="094e0-218">Modify the kernel boot line in your grub configuration to include additional kernel parameters for Azure.</span></span> <span data-ttu-id="094e0-219">Chcete-li provést tuto konfiguraci, otevřete `/boot/grub/menu.lst` v textovém editoru a ujistěte se, že výchozí jádra zahrnuje následující parametry:</span><span class="sxs-lookup"><span data-stu-id="094e0-219">To do this configuration, open `/boot/grub/menu.lst` in a text editor, and ensure that the default kernel includes the following parameters:</span></span>
    
        console=ttyS0 earlyprintk=ttyS0 rootdelay=300
    
    <span data-ttu-id="094e0-220">To také zajistí, že všechny zprávy konzoly odešlou do první sériového portu, který může být užitečné Azure podporu pro ladění problémů.</span><span class="sxs-lookup"><span data-stu-id="094e0-220">This will also ensure that all console messages are sent to the first serial port, which can assist Azure support with debugging issues.</span></span>
    
    <span data-ttu-id="094e0-221">Kromě toho doporučujeme odebrat následující parametry:</span><span class="sxs-lookup"><span data-stu-id="094e0-221">In addition, we recommend that you remove the following parameters:</span></span>
    
        rhgb quiet crashkernel=auto
    
    <span data-ttu-id="094e0-222">Grafické a quiet spouštěcí nejsou užitečné při cloudovém prostředí, kde chceme, aby všechny protokoly pro odeslání do sériového portu.</span><span class="sxs-lookup"><span data-stu-id="094e0-222">Graphical and quiet boot are not useful in a cloud environment where we want all the logs to be sent to the serial port.</span></span> <span data-ttu-id="094e0-223">Můžete nechat `crashkernel` možnost nakonfigurované v případě potřeby.</span><span class="sxs-lookup"><span data-stu-id="094e0-223">You can leave the `crashkernel` option configured if desired.</span></span> <span data-ttu-id="094e0-224">Všimněte si, že tento parametr snižuje množství dostupné paměti ve virtuálním počítači 128 MB a víc, což může způsobovat problémy menší velikostí virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="094e0-224">Note that this parameter reduces the amount of available memory in the virtual machine by 128 MB or more, which might be problematic on smaller virtual machine sizes.</span></span>

    >[!Important]
    <span data-ttu-id="094e0-225">RHEL verze 6.5 a starší, musíte taky nastavit `numa=off` jádra parametr.</span><span class="sxs-lookup"><span data-stu-id="094e0-225">RHEL 6.5 and earlier must also set the `numa=off` kernel parameter.</span></span> <span data-ttu-id="094e0-226">V tématu Red Hat [KB 436883](https://access.redhat.com/solutions/436883).</span><span class="sxs-lookup"><span data-stu-id="094e0-226">See Red Hat [KB 436883](https://access.redhat.com/solutions/436883).</span></span>

10. <span data-ttu-id="094e0-227">Přidání modulů technologie Hyper-V do initramfs:</span><span class="sxs-lookup"><span data-stu-id="094e0-227">Add Hyper-V modules to initramfs:</span></span>  

    <span data-ttu-id="094e0-228">Upravit `/etc/dracut.conf`a přidejte následující obsah:</span><span class="sxs-lookup"><span data-stu-id="094e0-228">Edit `/etc/dracut.conf`, and add the following content:</span></span>

        add_drivers+="hv_vmbus hv_netvsc hv_storvsc"

    <span data-ttu-id="094e0-229">Znovu sestavte initramfs:</span><span class="sxs-lookup"><span data-stu-id="094e0-229">Rebuild initramfs:</span></span>

        # dracut -f -v

11. <span data-ttu-id="094e0-230">Odinstalace init cloudu:</span><span class="sxs-lookup"><span data-stu-id="094e0-230">Uninstall cloud-init:</span></span>

        # yum remove cloud-init

12. <span data-ttu-id="094e0-231">Ujistěte se, že SSH server je nainstalován a nakonfigurován na spuštění při spuštění:</span><span class="sxs-lookup"><span data-stu-id="094e0-231">Ensure that the SSH server is installed and configured to start at boot time:</span></span>

        # chkconfig sshd on

    <span data-ttu-id="094e0-232">Upravte /etc/ssh/sshd_config zahrnuty následující řádky:</span><span class="sxs-lookup"><span data-stu-id="094e0-232">Modify /etc/ssh/sshd_config to include the following lines:</span></span>

        PasswordAuthentication yes
        ClientAliveInterval 180

13. <span data-ttu-id="094e0-233">Balíček WALinuxAgent `WALinuxAgent-<version>`, bylo posunuto do Red Hat funkce úložiště.</span><span class="sxs-lookup"><span data-stu-id="094e0-233">The WALinuxAgent package, `WALinuxAgent-<version>`, has been pushed to the Red Hat extras repository.</span></span> <span data-ttu-id="094e0-234">Povolení funkce úložiště tak, že spustíte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="094e0-234">Enable the extras repository by running the following command:</span></span>

        # subscription-manager repos --enable=rhel-6-server-extras-rpms

14. <span data-ttu-id="094e0-235">Nainstalujte Azure Linux Agent spuštěním následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="094e0-235">Install the Azure Linux Agent by running the following command:</span></span>

        # yum install WALinuxAgent

        # chkconfig waagent on

15. <span data-ttu-id="094e0-236">Azure Linux Agent mohou automaticky konfigurovat odkládacího souboru pomocí disku místní prostředek, který je připojen k virtuálnímu počítači po zřízení virtuálního počítače na platformě Azure.</span><span class="sxs-lookup"><span data-stu-id="094e0-236">The Azure Linux Agent can automatically configure swap space by using the local resource disk that is attached to the virtual machine after the virtual machine is provisioned on Azure.</span></span> <span data-ttu-id="094e0-237">Upozorňujeme, že disk místní prostředek je dočasný a může být vyprázdněny, když virtuální počítač je zrušit.</span><span class="sxs-lookup"><span data-stu-id="094e0-237">Note that the local resource disk is a temporary disk, and it might be emptied when the virtual machine is deprovisioned.</span></span> <span data-ttu-id="094e0-238">Po instalaci Azure Linux Agent v předchozím kroku, upravte následující parametry v **/etc/waagent.conf** odpovídajícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="094e0-238">After you install the Azure Linux Agent in the previous step, modify the following parameters in **/etc/waagent.conf** appropriately:</span></span>

        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this to whatever you need it to be.

16. <span data-ttu-id="094e0-239">Zrušení registrace předplatného (v případě potřeby) tak, že spustíte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="094e0-239">Unregister the subscription (if necessary) by running the following command:</span></span>

        # subscription-manager unregister

17. <span data-ttu-id="094e0-240">Spusťte následující příkaz pro zrušení zřízení virtuálního počítače a jeho přípravu pro zřizování na Azure:</span><span class="sxs-lookup"><span data-stu-id="094e0-240">Run the following commands to deprovision the virtual machine and prepare it for provisioning on Azure:</span></span>

        # waagent -force -deprovision

        # export HISTSIZE=0

        # logout

18. <span data-ttu-id="094e0-241">Vypnete virtuální počítač v KVM.</span><span class="sxs-lookup"><span data-stu-id="094e0-241">Shut down the virtual machine in KVM.</span></span>

19. <span data-ttu-id="094e0-242">Bitovou kopii qcow2 převeďte na formát VHD.</span><span class="sxs-lookup"><span data-stu-id="094e0-242">Convert the qcow2 image to the VHD format.</span></span>

    <span data-ttu-id="094e0-243">Nejdřív převeďte bitovou kopii formátu raw:</span><span class="sxs-lookup"><span data-stu-id="094e0-243">First convert the image to raw format:</span></span>

        # qemu-img convert -f qcow2 -O raw rhel-6.8.qcow2 rhel-6.8.raw

    <span data-ttu-id="094e0-244">Ujistěte se, že velikost nezpracovaná bitové kopie je v souladu s 1 MB.</span><span class="sxs-lookup"><span data-stu-id="094e0-244">Make sure that the size of the raw image is aligned with 1 MB.</span></span> <span data-ttu-id="094e0-245">Jinak zaokrouhlí nahoru na velikost zarovnané s 1 MB:</span><span class="sxs-lookup"><span data-stu-id="094e0-245">Otherwise, round up the size to align with 1 MB:</span></span>

        # MB=$((1024*1024))
        # size=$(qemu-img info -f raw --output json "rhel-6.8.raw" | \
          gawk 'match($0, /"virtual-size": ([0-9]+),/, val) {print val[1]}')

        # rounded_size=$((($size/$MB + 1)*$MB))
        # qemu-img resize rhel-6.8.raw $rounded_size

    <span data-ttu-id="094e0-246">Nezpracovaná disku převeďte na virtuální pevný disk pevné velikosti:</span><span class="sxs-lookup"><span data-stu-id="094e0-246">Convert the raw disk to a fixed-sized VHD:</span></span>

        # qemu-img convert -f raw -o subformat=fixed -O vpc rhel-6.8.raw rhel-6.8.vhd


### <a name="prepare-a-rhel-7-virtual-machine-from-kvm"></a><span data-ttu-id="094e0-247">Příprava virtuálního počítače, RHEL 7 z KVM</span><span class="sxs-lookup"><span data-stu-id="094e0-247">Prepare a RHEL 7 virtual machine from KVM</span></span>

1. <span data-ttu-id="094e0-248">Stáhněte z webu Red Hat KVM bitové kopie systému RHEL 7.</span><span class="sxs-lookup"><span data-stu-id="094e0-248">Download the KVM image of RHEL 7 from the Red Hat website.</span></span> <span data-ttu-id="094e0-249">Tento postup používá RHEL 7 jako v příkladu.</span><span class="sxs-lookup"><span data-stu-id="094e0-249">This procedure uses RHEL 7 as the example.</span></span>

2. <span data-ttu-id="094e0-250">Nastavení kořenové heslo.</span><span class="sxs-lookup"><span data-stu-id="094e0-250">Set a root password.</span></span>

    <span data-ttu-id="094e0-251">Generovat zašifrované heslo a zkopírujte výstup tohoto příkazu:</span><span class="sxs-lookup"><span data-stu-id="094e0-251">Generate an encrypted password, and copy the output of the command:</span></span>

        # openssl passwd -1 changeme

    <span data-ttu-id="094e0-252">Nastavení kořenové heslo s guestfish:</span><span class="sxs-lookup"><span data-stu-id="094e0-252">Set a root password with guestfish:</span></span>

        # guestfish --rw -a <image-name>
        > <fs> run
        > <fs> list-filesystems
        > <fs> mount /dev/sda1 /
        > <fs> vi /etc/shadow
        > <fs> exit

   <span data-ttu-id="094e0-253">Změňte druhé pole kořenové uživatele z "!"</span><span class="sxs-lookup"><span data-stu-id="094e0-253">Change the second field of root user from "!!"</span></span> <span data-ttu-id="094e0-254">do zašifrované heslo.</span><span class="sxs-lookup"><span data-stu-id="094e0-254">to the encrypted password.</span></span>

3. <span data-ttu-id="094e0-255">Vytvoření virtuálního počítače v KVM z qcow2 bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="094e0-255">Create a virtual machine in KVM from the qcow2 image.</span></span> <span data-ttu-id="094e0-256">Nastavte typ disku na **qcow2**a nastavte model zařízení rozhraní virtuální sítě na **virtio**.</span><span class="sxs-lookup"><span data-stu-id="094e0-256">Set the disk type to **qcow2**, and set the virtual network interface device model to **virtio**.</span></span> <span data-ttu-id="094e0-257">Potom spusťte virtuální počítač a přihlaste se jako kořenový.</span><span class="sxs-lookup"><span data-stu-id="094e0-257">Then, start the virtual machine, and sign in as root.</span></span>

4. <span data-ttu-id="094e0-258">Vytvořit nebo upravit `/etc/sysconfig/network` souboru a přidejte následující text:</span><span class="sxs-lookup"><span data-stu-id="094e0-258">Create or edit the `/etc/sysconfig/network` file, and add the following text:</span></span>
   
        NETWORKING=yes
        HOSTNAME=localhost.localdomain

5. <span data-ttu-id="094e0-259">Vytvořit nebo upravit `/etc/sysconfig/network-scripts/ifcfg-eth0` souboru a přidejte následující text:</span><span class="sxs-lookup"><span data-stu-id="094e0-259">Create or edit the `/etc/sysconfig/network-scripts/ifcfg-eth0` file, and add the following text:</span></span>
   
        DEVICE=eth0
        ONBOOT=yes
        BOOTPROTO=dhcp
        TYPE=Ethernet
        USERCTL=no
        PEERDNS=yes
        IPV6INIT=no
        NM_CONTROLLED=no

6. <span data-ttu-id="094e0-260">Ujistěte se, že síťové služby se spustí při spuštění spuštěním následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="094e0-260">Ensure that the network service will start at boot time by running the following command:</span></span>

        # chkconfig network on

7. <span data-ttu-id="094e0-261">Registrujte předplatné Red Hat povolit instalaci balíčků z úložiště RHEL spuštěním následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="094e0-261">Register your Red Hat subscription to enable installation of packages from the RHEL repository by running the following command:</span></span>

        # subscription-manager register --auto-attach --username=XXX --password=XXX

8. <span data-ttu-id="094e0-262">Upravte řádku spouštěcí jádra ve vaší konfiguraci grub pro zahrnutí dalších jádra parametry Azure.</span><span class="sxs-lookup"><span data-stu-id="094e0-262">Modify the kernel boot line in your grub configuration to include additional kernel parameters for Azure.</span></span> <span data-ttu-id="094e0-263">Chcete-li provést tuto konfiguraci, otevřete `/etc/default/grub` v textovém editoru a upravit `GRUB_CMDLINE_LINUX` parametr.</span><span class="sxs-lookup"><span data-stu-id="094e0-263">To do this configuration, open `/etc/default/grub` in a text editor, and edit the `GRUB_CMDLINE_LINUX` parameter.</span></span> <span data-ttu-id="094e0-264">Například:</span><span class="sxs-lookup"><span data-stu-id="094e0-264">For example:</span></span>
   
        GRUB_CMDLINE_LINUX="rootdelay=300 console=ttyS0 earlyprintk=ttyS0 net.ifnames=0"
   
   <span data-ttu-id="094e0-265">Tento příkaz také zajistí, že všechny zprávy konzoly odešlou do první sériového portu, který může být užitečné Azure podporu pro ladění problémů.</span><span class="sxs-lookup"><span data-stu-id="094e0-265">This command also ensures that all console messages are sent to the first serial port, which can assist Azure support with debugging issues.</span></span> <span data-ttu-id="094e0-266">Příkaz vypne také nové zásady vytváření názvů RHEL 7 pro síťové adaptéry.</span><span class="sxs-lookup"><span data-stu-id="094e0-266">The command also turns off the new RHEL 7 naming conventions for NICs.</span></span> <span data-ttu-id="094e0-267">Kromě toho doporučujeme odebrat následující parametry:</span><span class="sxs-lookup"><span data-stu-id="094e0-267">In addition, we recommend that you remove the following parameters:</span></span>
   
        rhgb quiet crashkernel=auto
   
    <span data-ttu-id="094e0-268">Grafické a quiet spouštěcí nejsou užitečné při cloudovém prostředí, kde chceme, aby všechny protokoly pro odeslání do sériového portu.</span><span class="sxs-lookup"><span data-stu-id="094e0-268">Graphical and quiet boot are not useful in a cloud environment where we want all the logs to be sent to the serial port.</span></span> <span data-ttu-id="094e0-269">Můžete nechat `crashkernel` možnost nakonfigurované v případě potřeby.</span><span class="sxs-lookup"><span data-stu-id="094e0-269">You can leave the `crashkernel` option configured if desired.</span></span> <span data-ttu-id="094e0-270">Všimněte si, že tento parametr snižuje množství dostupné paměti ve virtuálním počítači 128 MB a víc, což může způsobovat problémy menší velikostí virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="094e0-270">Note that this parameter reduces the amount of available memory in the virtual machine by 128 MB or more, which might be problematic on smaller virtual machine sizes.</span></span>

9. <span data-ttu-id="094e0-271">Po dokončení úprav `/etc/default/grub`, spusťte následující příkaz k opětovnému sestavení grub konfigurace:</span><span class="sxs-lookup"><span data-stu-id="094e0-271">After you are done editing `/etc/default/grub`, run the following command to rebuild the grub configuration:</span></span>

        # grub2-mkconfig -o /boot/grub2/grub.cfg

10. <span data-ttu-id="094e0-272">Přidejte do initramfs moduly Hyper-V.</span><span class="sxs-lookup"><span data-stu-id="094e0-272">Add Hyper-V modules into initramfs.</span></span>

    <span data-ttu-id="094e0-273">Upravit `/etc/dracut.conf` a přidejte obsah:</span><span class="sxs-lookup"><span data-stu-id="094e0-273">Edit `/etc/dracut.conf` and add content:</span></span>

        add_drivers+="hv_vmbus hv_netvsc hv_storvsc"

    <span data-ttu-id="094e0-274">Znovu sestavte initramfs:</span><span class="sxs-lookup"><span data-stu-id="094e0-274">Rebuild initramfs:</span></span>

        # dracut -f -v

11. <span data-ttu-id="094e0-275">Odinstalace init cloudu:</span><span class="sxs-lookup"><span data-stu-id="094e0-275">Uninstall cloud-init:</span></span>

        # yum remove cloud-init

12. <span data-ttu-id="094e0-276">Ujistěte se, že SSH server je nainstalován a nakonfigurován na spuštění při spuštění:</span><span class="sxs-lookup"><span data-stu-id="094e0-276">Ensure that the SSH server is installed and configured to start at boot time:</span></span>

        # systemctl enable sshd

    <span data-ttu-id="094e0-277">Upravte /etc/ssh/sshd_config zahrnuty následující řádky:</span><span class="sxs-lookup"><span data-stu-id="094e0-277">Modify /etc/ssh/sshd_config to include the following lines:</span></span>

        PasswordAuthentication yes
        ClientAliveInterval 180

13. <span data-ttu-id="094e0-278">Balíček WALinuxAgent `WALinuxAgent-<version>`, bylo posunuto do Red Hat funkce úložiště.</span><span class="sxs-lookup"><span data-stu-id="094e0-278">The WALinuxAgent package, `WALinuxAgent-<version>`, has been pushed to the Red Hat extras repository.</span></span> <span data-ttu-id="094e0-279">Povolení funkce úložiště tak, že spustíte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="094e0-279">Enable the extras repository by running the following command:</span></span>

        # subscription-manager repos --enable=rhel-7-server-extras-rpms

14. <span data-ttu-id="094e0-280">Nainstalujte Azure Linux Agent spuštěním následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="094e0-280">Install the Azure Linux Agent by running the following command:</span></span>

        # yum install WALinuxAgent

    <span data-ttu-id="094e0-281">Povolte službu příkaz waagent:</span><span class="sxs-lookup"><span data-stu-id="094e0-281">Enable the waagent service:</span></span>

        # systemctl enable waagent.service

15. <span data-ttu-id="094e0-282">Nevytvářejte odkládacího prostoru na disku operačního systému.</span><span class="sxs-lookup"><span data-stu-id="094e0-282">Do not create swap space on the operating system disk.</span></span>

    <span data-ttu-id="094e0-283">Azure Linux Agent mohou automaticky konfigurovat odkládacího souboru pomocí disku místní prostředek, který je připojen k virtuálnímu počítači po zřízení virtuálního počítače na platformě Azure.</span><span class="sxs-lookup"><span data-stu-id="094e0-283">The Azure Linux Agent can automatically configure swap space by using the local resource disk that is attached to the virtual machine after the virtual machine is provisioned on Azure.</span></span> <span data-ttu-id="094e0-284">Upozorňujeme, že disk místní prostředek je dočasný a může být vyprázdněny, když virtuální počítač je zrušit.</span><span class="sxs-lookup"><span data-stu-id="094e0-284">Note that the local resource disk is a temporary disk, and it might be emptied when the virtual machine is deprovisioned.</span></span> <span data-ttu-id="094e0-285">Po instalaci Azure Linux Agent v předchozím kroku, upravte následující parametry v `/etc/waagent.conf` odpovídajícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="094e0-285">After you install the Azure Linux Agent in the previous step, modify the following parameters in `/etc/waagent.conf` appropriately:</span></span>

        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this to whatever you need it to be.

16. <span data-ttu-id="094e0-286">Zrušení registrace předplatného (v případě potřeby) tak, že spustíte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="094e0-286">Unregister the subscription (if necessary) by running the following command:</span></span>

        # subscription-manager unregister

17. <span data-ttu-id="094e0-287">Spusťte následující příkaz pro zrušení zřízení virtuálního počítače a jeho přípravu pro zřizování na Azure:</span><span class="sxs-lookup"><span data-stu-id="094e0-287">Run the following commands to deprovision the virtual machine and prepare it for provisioning on Azure:</span></span>

        # sudo waagent -force -deprovision

        # export HISTSIZE=0

        # logout

18. <span data-ttu-id="094e0-288">Vypnete virtuální počítač v KVM.</span><span class="sxs-lookup"><span data-stu-id="094e0-288">Shut down the virtual machine in KVM.</span></span>

19. <span data-ttu-id="094e0-289">Bitovou kopii qcow2 převeďte na formát VHD.</span><span class="sxs-lookup"><span data-stu-id="094e0-289">Convert the qcow2 image to the VHD format.</span></span>

    <span data-ttu-id="094e0-290">Nejdřív převeďte bitovou kopii formátu raw:</span><span class="sxs-lookup"><span data-stu-id="094e0-290">First convert the image to raw format:</span></span>

        # qemu-img convert -f qcow2 -O raw rhel-7.3.qcow2 rhel-7.3.raw

    <span data-ttu-id="094e0-291">Ujistěte se, že velikost nezpracovaná bitové kopie je v souladu s 1 MB.</span><span class="sxs-lookup"><span data-stu-id="094e0-291">Make sure that the size of the raw image is aligned with 1 MB.</span></span> <span data-ttu-id="094e0-292">Jinak zaokrouhlí nahoru na velikost zarovnané s 1 MB:</span><span class="sxs-lookup"><span data-stu-id="094e0-292">Otherwise, round up the size to align with 1 MB:</span></span>

        # MB=$((1024*1024))
        # size=$(qemu-img info -f raw --output json "rhel-6.8.raw" | \
          gawk 'match($0, /"virtual-size": ([0-9]+),/, val) {print val[1]}')

        # rounded_size=$((($size/$MB + 1)*$MB))
        # qemu-img resize rhel-6.8.raw $rounded_size

    <span data-ttu-id="094e0-293">Nezpracovaná disku převeďte na virtuální pevný disk pevné velikosti:</span><span class="sxs-lookup"><span data-stu-id="094e0-293">Convert the raw disk to a fixed-sized VHD:</span></span>

        # qemu-img convert -f raw -o subformat=fixed -O vpc rhel-7.3.raw rhel-7.3.vhd

## <a name="prepare-a-red-hat-based-virtual-machine-from-vmware"></a><span data-ttu-id="094e0-294">Příprava virtuálního počítače, na základě Red Hat z VMware</span><span class="sxs-lookup"><span data-stu-id="094e0-294">Prepare a Red Hat-based virtual machine from VMware</span></span>
### <a name="prerequisites"></a><span data-ttu-id="094e0-295">Požadavky</span><span class="sxs-lookup"><span data-stu-id="094e0-295">Prerequisites</span></span>
<span data-ttu-id="094e0-296">V této části se předpokládá, že jste již nainstalovali RHEL virtuálního počítače v prostředí VMware.</span><span class="sxs-lookup"><span data-stu-id="094e0-296">This section assumes that you have already installed a RHEL virtual machine in VMware.</span></span> <span data-ttu-id="094e0-297">Podrobnosti o tom, jak nainstalovat operační systém v prostředí VMware najdete v tématu [Průvodce instalací operačního systému hosta VMware](http://partnerweb.vmware.com/GOSIG/home.html).</span><span class="sxs-lookup"><span data-stu-id="094e0-297">For details about how to install an operating system in VMware, see [VMware Guest Operating System Installation Guide](http://partnerweb.vmware.com/GOSIG/home.html).</span></span>

* <span data-ttu-id="094e0-298">Při instalaci operačního systému Linux, doporučujeme použít standardní oddíly spíše než LVM, což je často na výchozí hodnoty pro mnoho instalace.</span><span class="sxs-lookup"><span data-stu-id="094e0-298">When you install the Linux operating system, we recommend that you use standard partitions rather than LVM, which is often the default for many installations.</span></span> <span data-ttu-id="094e0-299">Tím se vyhnete LVM název je v konfliktu s naklonovaný virtuální počítač, zvlášť pokud někdy musí být připojen k jinému virtuálnímu počítači pro řešení potíží s diskem operačního systému.</span><span class="sxs-lookup"><span data-stu-id="094e0-299">This will avoid LVM name conflicts with cloned virtual machine, particularly if an operating system disk ever needs to be attached to another virtual machine for troubleshooting.</span></span> <span data-ttu-id="094e0-300">LVM nebo RAID lze použít v datových disků Pokud dáváte přednost.</span><span class="sxs-lookup"><span data-stu-id="094e0-300">LVM or RAID can be used on data disks if preferred.</span></span>
* <span data-ttu-id="094e0-301">Nekonfigurujte přepnutí oddílu na disku operačního systému.</span><span class="sxs-lookup"><span data-stu-id="094e0-301">Do not configure a swap partition on the operating system disk.</span></span> <span data-ttu-id="094e0-302">Můžete nakonfigurovat agenta systému Linux, chcete-li vytvořit odkládací soubor na disku dočasných prostředků.</span><span class="sxs-lookup"><span data-stu-id="094e0-302">You can configure the Linux agent to create a swap file on the temporary resource disk.</span></span> <span data-ttu-id="094e0-303">Další informace o tom najdete v krocích, které následují.</span><span class="sxs-lookup"><span data-stu-id="094e0-303">You can find more information about this in the steps that follow.</span></span>
* <span data-ttu-id="094e0-304">Když vytvoříte virtuální pevný disk, vyberte **virtuálního disku úložiště jako samostatný soubor**.</span><span class="sxs-lookup"><span data-stu-id="094e0-304">When you create the virtual hard disk, select **Store virtual disk as a single file**.</span></span>

### <a name="prepare-a-rhel-6-virtual-machine-from-vmware"></a><span data-ttu-id="094e0-305">Příprava systému RHEL 6 virtuálního počítače z VMware</span><span class="sxs-lookup"><span data-stu-id="094e0-305">Prepare a RHEL 6 virtual machine from VMware</span></span>
1. <span data-ttu-id="094e0-306">V systému RHEL 6 NetworkManager může narušovat Azure Linux agent.</span><span class="sxs-lookup"><span data-stu-id="094e0-306">In RHEL 6, NetworkManager can interfere with the Azure Linux agent.</span></span> <span data-ttu-id="094e0-307">Odinstalaci tohoto balíčku tak, že spustíte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="094e0-307">Uninstall this package by running the following command:</span></span>
   
        # sudo rpm -e --nodeps NetworkManager

2. <span data-ttu-id="094e0-308">Vytvořte soubor s názvem **sítě** v etc/sysconfig/adresář, který obsahuje následující text:</span><span class="sxs-lookup"><span data-stu-id="094e0-308">Create a file named **network** in the /etc/sysconfig/ directory that contains the following text:</span></span>

        NETWORKING=yes
        HOSTNAME=localhost.localdomain

3. <span data-ttu-id="094e0-309">Vytvořit nebo upravit `/etc/sysconfig/network-scripts/ifcfg-eth0` souboru a přidejte následující text:</span><span class="sxs-lookup"><span data-stu-id="094e0-309">Create or edit the `/etc/sysconfig/network-scripts/ifcfg-eth0` file, and add the following text:</span></span>
   
        DEVICE=eth0
        ONBOOT=yes
        BOOTPROTO=dhcp
        TYPE=Ethernet
        USERCTL=no
        PEERDNS=yes
        IPV6INIT=no

4. <span data-ttu-id="094e0-310">(Nebo odebrat) udev pravidel tak, aby nedošlo ke generování statická pravidla pro rozhraní sítě Ethernet.</span><span class="sxs-lookup"><span data-stu-id="094e0-310">Move (or remove) the udev rules to avoid generating static rules for the Ethernet interface.</span></span> <span data-ttu-id="094e0-311">Tato pravidla způsobit problémy při klonování virtuálního počítače v Azure nebo Hyper-V:</span><span class="sxs-lookup"><span data-stu-id="094e0-311">These rules cause problems when you clone a virtual machine in Azure or Hyper-V:</span></span>

        # sudo ln -s /dev/null /etc/udev/rules.d/75-persistent-net-generator.rules

        # sudo rm -f /etc/udev/rules.d/70-persistent-net.rules

5. <span data-ttu-id="094e0-312">Ujistěte se, že síťové služby se spustí při spuštění spuštěním následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="094e0-312">Ensure that the network service will start at boot time by running the following command:</span></span>

        # sudo chkconfig network on

6. <span data-ttu-id="094e0-313">Registrujte předplatné Red Hat povolit instalaci balíčků z úložiště RHEL spuštěním následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="094e0-313">Register your Red Hat subscription to enable the installation of packages from the RHEL repository by running the following command:</span></span>

        # sudo subscription-manager register --auto-attach --username=XXX --password=XXX

7. <span data-ttu-id="094e0-314">Balíček WALinuxAgent `WALinuxAgent-<version>`, bylo posunuto do Red Hat funkce úložiště.</span><span class="sxs-lookup"><span data-stu-id="094e0-314">The WALinuxAgent package, `WALinuxAgent-<version>`, has been pushed to the Red Hat extras repository.</span></span> <span data-ttu-id="094e0-315">Povolení funkce úložiště tak, že spustíte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="094e0-315">Enable the extras repository by running the following command:</span></span>

        # subscription-manager repos --enable=rhel-6-server-extras-rpms

8. <span data-ttu-id="094e0-316">Upravte řádku spouštěcí jádra ve vaší konfiguraci grub pro zahrnutí dalších jádra parametry Azure.</span><span class="sxs-lookup"><span data-stu-id="094e0-316">Modify the kernel boot line in your grub configuration to include additional kernel parameters for Azure.</span></span> <span data-ttu-id="094e0-317">Chcete-li to provést, otevřete `/etc/default/grub` v textovém editoru a upravit `GRUB_CMDLINE_LINUX` parametr.</span><span class="sxs-lookup"><span data-stu-id="094e0-317">To do this, open `/etc/default/grub` in a text editor, and edit the `GRUB_CMDLINE_LINUX` parameter.</span></span> <span data-ttu-id="094e0-318">Například:</span><span class="sxs-lookup"><span data-stu-id="094e0-318">For example:</span></span>
   
        GRUB_CMDLINE_LINUX="rootdelay=300 console=ttyS0 earlyprintk=ttyS0"
   
   <span data-ttu-id="094e0-319">To také zajistí, že všechny zprávy konzoly odešlou do první sériového portu, který může být užitečné Azure podporu pro ladění problémů.</span><span class="sxs-lookup"><span data-stu-id="094e0-319">This will also ensure that all console messages are sent to the first serial port, which can assist Azure support with debugging issues.</span></span> <span data-ttu-id="094e0-320">Kromě toho doporučujeme odebrat následující parametry:</span><span class="sxs-lookup"><span data-stu-id="094e0-320">In addition, we recommend that you remove the following parameters:</span></span>
   
        rhgb quiet crashkernel=auto
   
    <span data-ttu-id="094e0-321">Grafické a quiet spouštěcí nejsou užitečné při cloudovém prostředí, kde chceme, aby všechny protokoly pro odeslání do sériového portu.</span><span class="sxs-lookup"><span data-stu-id="094e0-321">Graphical and quiet boot are not useful in a cloud environment where we want all the logs to be sent to the serial port.</span></span> <span data-ttu-id="094e0-322">Můžete nechat `crashkernel` možnost nakonfigurované v případě potřeby.</span><span class="sxs-lookup"><span data-stu-id="094e0-322">You can leave the `crashkernel` option configured if desired.</span></span> <span data-ttu-id="094e0-323">Všimněte si, že tento parametr snižuje množství dostupné paměti ve virtuálním počítači 128 MB a víc, což může způsobovat problémy menší velikostí virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="094e0-323">Note that this parameter reduces the amount of available memory in the virtual machine by 128 MB or more, which might be problematic on smaller virtual machine sizes.</span></span>

9. <span data-ttu-id="094e0-324">Přidání modulů technologie Hyper-V do initramfs:</span><span class="sxs-lookup"><span data-stu-id="094e0-324">Add Hyper-V modules to initramfs:</span></span>

    <span data-ttu-id="094e0-325">Upravit `/etc/dracut.conf`a přidejte následující obsah:</span><span class="sxs-lookup"><span data-stu-id="094e0-325">Edit `/etc/dracut.conf`, and add the following content:</span></span>

        add_drivers+="hv_vmbus hv_netvsc hv_storvsc"

    <span data-ttu-id="094e0-326">Znovu sestavte initramfs:</span><span class="sxs-lookup"><span data-stu-id="094e0-326">Rebuild initramfs:</span></span>

        # dracut -f -v

10. <span data-ttu-id="094e0-327">Zajistěte, aby serverem SSH je nainstalován a nakonfigurován na spuštění při spuštění, což obvykle výchozí nastavení.</span><span class="sxs-lookup"><span data-stu-id="094e0-327">Ensure that the SSH server is installed and configured to start at boot time, which is usually the default.</span></span> <span data-ttu-id="094e0-328">Upravit `/etc/ssh/sshd_config` obsahovala následující řádek:</span><span class="sxs-lookup"><span data-stu-id="094e0-328">Modify `/etc/ssh/sshd_config` to include the following line:</span></span>

    <span data-ttu-id="094e0-329">ClientAliveInterval 180</span><span class="sxs-lookup"><span data-stu-id="094e0-329">ClientAliveInterval 180</span></span>

11. <span data-ttu-id="094e0-330">Nainstalujte Azure Linux Agent spuštěním následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="094e0-330">Install the Azure Linux Agent by running the following command:</span></span>

        # sudo yum install WALinuxAgent

        # sudo chkconfig waagent on

12. <span data-ttu-id="094e0-331">Nevytvářejte odkládacího prostoru na disku operačního systému.</span><span class="sxs-lookup"><span data-stu-id="094e0-331">Do not create swap space on the operating system disk.</span></span>

    <span data-ttu-id="094e0-332">Azure Linux Agent mohou automaticky konfigurovat odkládacího souboru pomocí disku místní prostředek, který je připojen k virtuálnímu počítači po zřízení virtuálního počítače na platformě Azure.</span><span class="sxs-lookup"><span data-stu-id="094e0-332">The Azure Linux Agent can automatically configure swap space by using the local resource disk that is attached to the virtual machine after the virtual machine is provisioned on Azure.</span></span> <span data-ttu-id="094e0-333">Upozorňujeme, že disk místní prostředek je dočasný a může být vyprázdněny, když virtuální počítač je zrušit.</span><span class="sxs-lookup"><span data-stu-id="094e0-333">Note that the local resource disk is a temporary disk, and it might be emptied when the virtual machine is deprovisioned.</span></span> <span data-ttu-id="094e0-334">Po instalaci Azure Linux Agent v předchozím kroku, upravte následující parametry v `/etc/waagent.conf` odpovídajícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="094e0-334">After you install the Azure Linux Agent in the previous step, modify the following parameters in `/etc/waagent.conf` appropriately:</span></span>

        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this to whatever you need it to be.

13. <span data-ttu-id="094e0-335">Zrušení registrace předplatného (v případě potřeby) tak, že spustíte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="094e0-335">Unregister the subscription (if necessary) by running the following command:</span></span>

        # sudo subscription-manager unregister

14. <span data-ttu-id="094e0-336">Spusťte následující příkaz pro zrušení zřízení virtuálního počítače a jeho přípravu pro zřizování na Azure:</span><span class="sxs-lookup"><span data-stu-id="094e0-336">Run the following commands to deprovision the virtual machine and prepare it for provisioning on Azure:</span></span>

        # sudo waagent -force -deprovision

        # export HISTSIZE=0

        # logout

15. <span data-ttu-id="094e0-337">Vypněte virtuální počítač a převeďte soubor VMDK na soubor VHD.</span><span class="sxs-lookup"><span data-stu-id="094e0-337">Shut down the virtual machine, and convert the VMDK file to a .vhd file.</span></span>

    <span data-ttu-id="094e0-338">Nejdřív převeďte bitovou kopii formátu raw:</span><span class="sxs-lookup"><span data-stu-id="094e0-338">First convert the image to raw format:</span></span>

        # qemu-img convert -f vmdk -O raw rhel-6.8.vmdk rhel-6.8.raw

    <span data-ttu-id="094e0-339">Ujistěte se, že velikost nezpracovaná bitové kopie je v souladu s 1 MB.</span><span class="sxs-lookup"><span data-stu-id="094e0-339">Make sure that the size of the raw image is aligned with 1 MB.</span></span> <span data-ttu-id="094e0-340">Jinak zaokrouhlí nahoru na velikost zarovnané s 1 MB:</span><span class="sxs-lookup"><span data-stu-id="094e0-340">Otherwise, round up the size to align with 1 MB:</span></span>

        # MB=$((1024*1024))
        # size=$(qemu-img info -f raw --output json "rhel-6.8.raw" | \
          gawk 'match($0, /"virtual-size": ([0-9]+),/, val) {print val[1]}')

        # rounded_size=$((($size/$MB + 1)*$MB))
        # qemu-img resize rhel-6.8.raw $rounded_size

    <span data-ttu-id="094e0-341">Nezpracovaná disku převeďte na virtuální pevný disk pevné velikosti:</span><span class="sxs-lookup"><span data-stu-id="094e0-341">Convert the raw disk to a fixed-sized VHD:</span></span>

        # qemu-img convert -f raw -o subformat=fixed -O vpc rhel-6.8.raw rhel-6.8.vhd

### <a name="prepare-a-rhel-7-virtual-machine-from-vmware"></a><span data-ttu-id="094e0-342">Příprava virtuálního počítače, RHEL 7 z VMware</span><span class="sxs-lookup"><span data-stu-id="094e0-342">Prepare a RHEL 7 virtual machine from VMware</span></span>
1. <span data-ttu-id="094e0-343">Vytvořit nebo upravit `/etc/sysconfig/network` souboru a přidejte následující text:</span><span class="sxs-lookup"><span data-stu-id="094e0-343">Create or edit the `/etc/sysconfig/network` file, and add the following text:</span></span>
   
        NETWORKING=yes
        HOSTNAME=localhost.localdomain

2. <span data-ttu-id="094e0-344">Vytvořit nebo upravit `/etc/sysconfig/network-scripts/ifcfg-eth0` souboru a přidejte následující text:</span><span class="sxs-lookup"><span data-stu-id="094e0-344">Create or edit the `/etc/sysconfig/network-scripts/ifcfg-eth0` file, and add the following text:</span></span>
   
        DEVICE=eth0
        ONBOOT=yes
        BOOTPROTO=dhcp
        TYPE=Ethernet
        USERCTL=no
        PEERDNS=yes
        IPV6INIT=no
        NM_CONTROLLED=no

3. <span data-ttu-id="094e0-345">Ujistěte se, že síťové služby se spustí při spuštění spuštěním následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="094e0-345">Ensure that the network service will start at boot time by running the following command:</span></span>

        # sudo chkconfig network on

4. <span data-ttu-id="094e0-346">Registrujte předplatné Red Hat povolit instalaci balíčků z úložiště RHEL spuštěním následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="094e0-346">Register your Red Hat subscription to enable the installation of packages from the RHEL repository by running the following command:</span></span>

        # sudo subscription-manager register --auto-attach --username=XXX --password=XXX

5. <span data-ttu-id="094e0-347">Upravte řádku spouštěcí jádra ve vaší konfiguraci grub pro zahrnutí dalších jádra parametry Azure.</span><span class="sxs-lookup"><span data-stu-id="094e0-347">Modify the kernel boot line in your grub configuration to include additional kernel parameters for Azure.</span></span> <span data-ttu-id="094e0-348">Chcete-li provést tuto změny, otevřete `/etc/default/grub` v textovém editoru a upravit `GRUB_CMDLINE_LINUX` parametr.</span><span class="sxs-lookup"><span data-stu-id="094e0-348">To do this modification, open `/etc/default/grub` in a text editor, and edit the `GRUB_CMDLINE_LINUX` parameter.</span></span> <span data-ttu-id="094e0-349">Například:</span><span class="sxs-lookup"><span data-stu-id="094e0-349">For example:</span></span>
   
        GRUB_CMDLINE_LINUX="rootdelay=300 console=ttyS0 earlyprintk=ttyS0 net.ifnames=0"
   
   <span data-ttu-id="094e0-350">Tato konfigurace taky zajišťuje, že všechny zprávy konzoly odešlou do první sériového portu, který může být užitečné Azure podporu pro ladění problémů.</span><span class="sxs-lookup"><span data-stu-id="094e0-350">This configuration also ensures that all console messages are sent to the first serial port, which can assist Azure support with debugging issues.</span></span> <span data-ttu-id="094e0-351">Je také vypne nové zásady vytváření názvů RHEL 7 pro síťové adaptéry.</span><span class="sxs-lookup"><span data-stu-id="094e0-351">It also turns off the new RHEL 7 naming conventions for NICs.</span></span> <span data-ttu-id="094e0-352">Kromě toho doporučujeme odebrat následující parametry:</span><span class="sxs-lookup"><span data-stu-id="094e0-352">In addition, we recommend that you remove the following parameters:</span></span>
   
        rhgb quiet crashkernel=auto
   
    <span data-ttu-id="094e0-353">Grafické a quiet spouštěcí nejsou užitečné při cloudovém prostředí, kde chceme, aby všechny protokoly pro odeslání do sériového portu.</span><span class="sxs-lookup"><span data-stu-id="094e0-353">Graphical and quiet boot are not useful in a cloud environment where we want all the logs to be sent to the serial port.</span></span> <span data-ttu-id="094e0-354">Můžete nechat `crashkernel` možnost nakonfigurované v případě potřeby.</span><span class="sxs-lookup"><span data-stu-id="094e0-354">You can leave the `crashkernel` option configured if desired.</span></span> <span data-ttu-id="094e0-355">Všimněte si, že tento parametr snižuje množství dostupné paměti ve virtuálním počítači 128 MB a víc, což může způsobovat problémy menší velikostí virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="094e0-355">Note that this parameter reduces the amount of available memory in the virtual machine by 128 MB or more, which might be problematic on smaller virtual machine sizes.</span></span>

6. <span data-ttu-id="094e0-356">Po dokončení úprav `/etc/default/grub`, spusťte následující příkaz k opětovnému sestavení grub konfigurace:</span><span class="sxs-lookup"><span data-stu-id="094e0-356">After you are done editing `/etc/default/grub`, run the following command to rebuild the grub configuration:</span></span>

        # sudo grub2-mkconfig -o /boot/grub2/grub.cfg

7. <span data-ttu-id="094e0-357">Přidejte do initramfs moduly Hyper-V.</span><span class="sxs-lookup"><span data-stu-id="094e0-357">Add Hyper-V modules to initramfs.</span></span>

    <span data-ttu-id="094e0-358">Upravit `/etc/dracut.conf`, přidejte obsah:</span><span class="sxs-lookup"><span data-stu-id="094e0-358">Edit `/etc/dracut.conf`, add content:</span></span>

        add_drivers+="hv_vmbus hv_netvsc hv_storvsc"

    <span data-ttu-id="094e0-359">Znovu sestavte initramfs:</span><span class="sxs-lookup"><span data-stu-id="094e0-359">Rebuild initramfs:</span></span>

        # dracut -f -v

8. <span data-ttu-id="094e0-360">Ujistěte se, že SSH server je nainstalován a nakonfigurován na spuštění při spuštění.</span><span class="sxs-lookup"><span data-stu-id="094e0-360">Ensure that the SSH server is installed and configured to start at boot time.</span></span> <span data-ttu-id="094e0-361">Toto nastavení je obvykle výchozí.</span><span class="sxs-lookup"><span data-stu-id="094e0-361">This setting is usually the default.</span></span> <span data-ttu-id="094e0-362">Upravit `/etc/ssh/sshd_config` obsahovala následující řádek:</span><span class="sxs-lookup"><span data-stu-id="094e0-362">Modify `/etc/ssh/sshd_config` to include the following line:</span></span>

        ClientAliveInterval 180

9. <span data-ttu-id="094e0-363">Balíček WALinuxAgent `WALinuxAgent-<version>`, bylo posunuto do Red Hat funkce úložiště.</span><span class="sxs-lookup"><span data-stu-id="094e0-363">The WALinuxAgent package, `WALinuxAgent-<version>`, has been pushed to the Red Hat extras repository.</span></span> <span data-ttu-id="094e0-364">Povolení funkce úložiště tak, že spustíte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="094e0-364">Enable the extras repository by running the following command:</span></span>

        # subscription-manager repos --enable=rhel-7-server-extras-rpms

10. <span data-ttu-id="094e0-365">Nainstalujte Azure Linux Agent spuštěním následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="094e0-365">Install the Azure Linux Agent by running the following command:</span></span>

        # sudo yum install WALinuxAgent

        # sudo systemctl enable waagent.service

11. <span data-ttu-id="094e0-366">Nevytvářejte odkládacího prostoru na disku operačního systému.</span><span class="sxs-lookup"><span data-stu-id="094e0-366">Do not create swap space on the operating system disk.</span></span>

    <span data-ttu-id="094e0-367">Azure Linux Agent mohou automaticky konfigurovat odkládacího souboru pomocí disku místní prostředek, který je připojen k virtuálnímu počítači po zřízení virtuálního počítače na platformě Azure.</span><span class="sxs-lookup"><span data-stu-id="094e0-367">The Azure Linux Agent can automatically configure swap space by using the local resource disk that is attached to the virtual machine after the virtual machine is provisioned on Azure.</span></span> <span data-ttu-id="094e0-368">Upozorňujeme, že disk místní prostředek je dočasný a může být vyprázdněny, když virtuální počítač je zrušit.</span><span class="sxs-lookup"><span data-stu-id="094e0-368">Note that the local resource disk is a temporary disk, and it might be emptied when the virtual machine is deprovisioned.</span></span> <span data-ttu-id="094e0-369">Po instalaci Azure Linux Agent v předchozím kroku, upravte následující parametry v `/etc/waagent.conf` odpovídajícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="094e0-369">After you install the Azure Linux Agent in the previous step, modify the following parameters in `/etc/waagent.conf` appropriately:</span></span>

        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this to whatever you need it to be.

12. <span data-ttu-id="094e0-370">Pokud chcete zrušit registraci předplatného, spusťte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="094e0-370">If you want to unregister the subscription, run the following command:</span></span>

        # sudo subscription-manager unregister

13. <span data-ttu-id="094e0-371">Spusťte následující příkaz pro zrušení zřízení virtuálního počítače a jeho přípravu pro zřizování na Azure:</span><span class="sxs-lookup"><span data-stu-id="094e0-371">Run the following commands to deprovision the virtual machine and prepare it for provisioning on Azure:</span></span>

        # sudo waagent -force -deprovision

        # export HISTSIZE=0

        # logout

14. <span data-ttu-id="094e0-372">Vypněte virtuální počítač a převeďte soubor VMDK na formát VHD.</span><span class="sxs-lookup"><span data-stu-id="094e0-372">Shut down the virtual machine, and convert the VMDK file to the VHD format.</span></span>

    <span data-ttu-id="094e0-373">Nejdřív převeďte bitovou kopii formátu raw:</span><span class="sxs-lookup"><span data-stu-id="094e0-373">First convert the image to raw format:</span></span>

        # qemu-img convert -f vmdk -O raw rhel-7.3.vmdk rhel-7.3.raw

    <span data-ttu-id="094e0-374">Ujistěte se, že velikost nezpracovaná bitové kopie je v souladu s 1 MB.</span><span class="sxs-lookup"><span data-stu-id="094e0-374">Make sure that the size of the raw image is aligned with 1 MB.</span></span> <span data-ttu-id="094e0-375">Jinak zaokrouhlí nahoru na velikost zarovnané s 1 MB:</span><span class="sxs-lookup"><span data-stu-id="094e0-375">Otherwise, round up the size to align with 1 MB:</span></span>

        # MB=$((1024*1024))
        # size=$(qemu-img info -f raw --output json "rhel-6.8.raw" | \
          gawk 'match($0, /"virtual-size": ([0-9]+),/, val) {print val[1]}')

        # rounded_size=$((($size/$MB + 1)*$MB))
        # qemu-img resize rhel-6.8.raw $rounded_size

    <span data-ttu-id="094e0-376">Nezpracovaná disku převeďte na virtuální pevný disk pevné velikosti:</span><span class="sxs-lookup"><span data-stu-id="094e0-376">Convert the raw disk to a fixed-sized VHD:</span></span>

        # qemu-img convert -f raw -o subformat=fixed -O vpc rhel-7.3.raw rhel-7.3.vhd

## <a name="prepare-a-red-hat-based-virtual-machine-from-an-iso-by-using-a-kickstart-file-automatically"></a><span data-ttu-id="094e0-377">Příprava virtuálního počítače, na základě Red Hat ze souboru ISO pomocí souboru kickstart automaticky</span><span class="sxs-lookup"><span data-stu-id="094e0-377">Prepare a Red Hat-based virtual machine from an ISO by using a kickstart file automatically</span></span>
### <a name="prepare-a-rhel-7-virtual-machine-from-a-kickstart-file"></a><span data-ttu-id="094e0-378">Příprava virtuálního počítače ze souboru kickstart RHEL 7</span><span class="sxs-lookup"><span data-stu-id="094e0-378">Prepare a RHEL 7 virtual machine from a kickstart file</span></span>

1.  <span data-ttu-id="094e0-379">Vytvořte soubor kickstart, který obsahuje následující obsah a uložte soubor.</span><span class="sxs-lookup"><span data-stu-id="094e0-379">Create a kickstart file that includes the following content, and save the file.</span></span> <span data-ttu-id="094e0-380">Podrobnosti o kickstart instalace najdete v tématu [Průvodce instalací Kickstart](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/7/html/Installation_Guide/chap-kickstart-installations.html).</span><span class="sxs-lookup"><span data-stu-id="094e0-380">For details about kickstart installation, see the [Kickstart Installation Guide](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/7/html/Installation_Guide/chap-kickstart-installations.html).</span></span>

        # Kickstart for provisioning a RHEL 7 Azure VM

        # System authorization information
          auth --enableshadow --passalgo=sha512

        # Use graphical install
        text

        # Do not run the Setup Agent on first boot
        firstboot --disable

        # Keyboard layouts
        keyboard --vckeymap=us --xlayouts='us'

        # System language
        lang en_US.UTF-8

        # Network information
        network  --bootproto=dhcp

        # Root password
        rootpw --plaintext "to_be_disabled"

        # System services
        services --enabled="sshd,waagent,NetworkManager"

        # System timezone
        timezone Etc/UTC --isUtc --ntpservers 0.rhel.pool.ntp.org,1.rhel.pool.ntp.org,2.rhel.pool.ntp.org,3.rhel.pool.ntp.org

        # Partition clearing information
        clearpart --all --initlabel

        # Clear the MBR
        zerombr

        # Disk partitioning information
        part /boot --fstype="xfs" --size=500
        part / --fstyp="xfs" --size=1 --grow --asprimary

        # System bootloader configuration
        bootloader --location=mbr

        # Firewall configuration
        firewall --disabled

        # Enable SELinux
        selinux --enforcing

        # Don't configure X
        skipx

        # Power down the machine after install
        poweroff

        %packages
        @base
        @console-internet
        chrony
        sudo
        parted
        -dracut-config-rescue

        %end

        %post --log=/var/log/anaconda/post-install.log

        #!/bin/bash

        # Register Red Hat Subscription
        subscription-manager register --username=XXX --password=XXX --auto-attach --force

        # Install latest repo update
        yum update -y

        # Enable extras repo
        subscription-manager repos --enable=rhel-7-server-extras-rpms

        # Install WALinuxAgent
        yum install -y WALinuxAgent

        # Unregister Red Hat subscription
        subscription-manager unregister

        # Enable waaagent at boot-up
        systemctl enable waagent

        # Disable the root account
        usermod root -p '!!'

        # Configure swap in WALinuxAgent
        sed -i 's/^\(ResourceDisk\.EnableSwap\)=[Nn]$/\1=y/g' /etc/waagent.conf
        sed -i 's/^\(ResourceDisk\.SwapSizeMB\)=[0-9]*$/\1=2048/g' /etc/waagent.conf

        # Set the cmdline
        sed -i 's/^\(GRUB_CMDLINE_LINUX\)=".*"$/\1="console=tty1 console=ttyS0 earlyprintk=ttyS0 rootdelay=300"/g' /etc/default/grub

        # Enable SSH keepalive
        sed -i 's/^#\(ClientAliveInterval\).*$/\1 180/g' /etc/ssh/sshd_config

        # Build the grub cfg
        grub2-mkconfig -o /boot/grub2/grub.cfg

        # Configure network
        cat << EOF > /etc/sysconfig/network-scripts/ifcfg-eth0
        DEVICE=eth0
        ONBOOT=yes
        BOOTPROTO=dhcp
        TYPE=Ethernet
        USERCTL=no
        PEERDNS=yes
        IPV6INIT=no
        NM_CONTROLLED=no
        EOF

        # Deprovision and prepare for Azure
        waagent -force -deprovision

        %end

2. <span data-ttu-id="094e0-381">Umístěte soubor kickstart, kde systém instalace k němu přístup.</span><span class="sxs-lookup"><span data-stu-id="094e0-381">Place the kickstart file where the installation system can access it.</span></span>

3. <span data-ttu-id="094e0-382">Ve Správci technologie Hyper-V vytvořte nový virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="094e0-382">In Hyper-V Manager, create a new virtual machine.</span></span> <span data-ttu-id="094e0-383">Na **připojit virtuální pevný Disk** vyberte **připojit virtuální pevný disk později**a dokončete Průvodce novým virtuálním počítačem.</span><span class="sxs-lookup"><span data-stu-id="094e0-383">On the **Connect Virtual Hard Disk** page, select **Attach a virtual hard disk later**, and complete the New Virtual Machine Wizard.</span></span>

4. <span data-ttu-id="094e0-384">Otevřete nastavení virtuálního počítače:</span><span class="sxs-lookup"><span data-stu-id="094e0-384">Open the virtual machine settings:</span></span>

    <span data-ttu-id="094e0-385">a.</span><span class="sxs-lookup"><span data-stu-id="094e0-385">a.</span></span>  <span data-ttu-id="094e0-386">Připojte nový virtuální pevný disk k virtuálnímu počítači.</span><span class="sxs-lookup"><span data-stu-id="094e0-386">Attach a new virtual hard disk to the virtual machine.</span></span> <span data-ttu-id="094e0-387">Je nutné vybrat **formátu virtuálního pevného disku** a **pevné velikosti**.</span><span class="sxs-lookup"><span data-stu-id="094e0-387">Make sure to select **VHD Format** and **Fixed Size**.</span></span>

    <span data-ttu-id="094e0-388">b.</span><span class="sxs-lookup"><span data-stu-id="094e0-388">b.</span></span>  <span data-ttu-id="094e0-389">Připojte instalace ISO k jednotce DVD.</span><span class="sxs-lookup"><span data-stu-id="094e0-389">Attach the installation ISO to the DVD drive.</span></span>

    <span data-ttu-id="094e0-390">c.</span><span class="sxs-lookup"><span data-stu-id="094e0-390">c.</span></span>  <span data-ttu-id="094e0-391">Nastavení systému BIOS spouštění z disku CD.</span><span class="sxs-lookup"><span data-stu-id="094e0-391">Set the BIOS to boot from CD.</span></span>

5. <span data-ttu-id="094e0-392">Umožňuje spustit virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="094e0-392">Start the virtual machine.</span></span> <span data-ttu-id="094e0-393">Jakmile se zobrazí v instalační příručce, stiskněte klávesu **kartě** nakonfigurovat možnosti spuštění.</span><span class="sxs-lookup"><span data-stu-id="094e0-393">When the installation guide appears, press **Tab** to configure the boot options.</span></span>

6. <span data-ttu-id="094e0-394">Zadejte `inst.ks=<the location of the kickstart file>` na konci možnosti spuštění a stiskněte klávesu **Enter**.</span><span class="sxs-lookup"><span data-stu-id="094e0-394">Enter `inst.ks=<the location of the kickstart file>` at the end of the boot options, and press **Enter**.</span></span>

7. <span data-ttu-id="094e0-395">Počkejte na dokončení instalace.</span><span class="sxs-lookup"><span data-stu-id="094e0-395">Wait for the installation to finish.</span></span> <span data-ttu-id="094e0-396">Po dokončení, virtuální počítač se vypne automaticky.</span><span class="sxs-lookup"><span data-stu-id="094e0-396">When it's finished, the virtual machine will be shut down automatically.</span></span> <span data-ttu-id="094e0-397">Svůj disk VHD Linux je nyní připravena k odeslání do Azure.</span><span class="sxs-lookup"><span data-stu-id="094e0-397">Your Linux VHD is now ready to be uploaded to Azure.</span></span>

## <a name="known-issues"></a><span data-ttu-id="094e0-398">Známé problémy</span><span class="sxs-lookup"><span data-stu-id="094e0-398">Known issues</span></span>
### <a name="the-hyper-v-driver-could-not-be-included-in-the-initial-ram-disk-when-using-a-non-hyper-v-hypervisor"></a><span data-ttu-id="094e0-399">Ovladač technologie Hyper-V není v počáteční disku paměti RAM při použití hypervisor bez technologie Hyper-V</span><span class="sxs-lookup"><span data-stu-id="094e0-399">The Hyper-V driver could not be included in the initial RAM disk when using a non-Hyper-V hypervisor</span></span>

<span data-ttu-id="094e0-400">V některých případech instalačních programů Linux nemusí zahrnovat ovladače pro Hyper-V na počáteční disku paměti RAM (initrd nebo initramfs) Pokud Linux zjistí, zda je spuštěna v prostředí Hyper-V.</span><span class="sxs-lookup"><span data-stu-id="094e0-400">In some cases, Linux installers might not include the drivers for Hyper-V in the initial RAM disk (initrd or initramfs) unless Linux detects that it is running in a Hyper-V environment.</span></span>

<span data-ttu-id="094e0-401">Pokud používáte jiný virtualizační systému (tj. Virtualbox, Xen atd.) Příprava bitové kopie systému Linux, možná budete muset znovu vytvořit initrd zajistit, aby aspoň jádra modulů hv_vmbus a hv_storvsc jsou k dispozici na počáteční disku paměti RAM.</span><span class="sxs-lookup"><span data-stu-id="094e0-401">When you're using a different virtualization system (that is, Virtualbox, Xen, etc.) to prepare your Linux image, you might need to rebuild initrd to ensure that at least the hv_vmbus and hv_storvsc kernel modules are available on the initial RAM disk.</span></span> <span data-ttu-id="094e0-402">V systémech, které jsou založeny na nadřazený distribuce Red Hat alespoň jde o známý problém.</span><span class="sxs-lookup"><span data-stu-id="094e0-402">This is a known issue at least on systems that are based on the upstream Red Hat distribution.</span></span>

<span data-ttu-id="094e0-403">Chcete-li vyřešit tento problém, přidejte do initramfs moduly Hyper-V a znovu sestavte jej:</span><span class="sxs-lookup"><span data-stu-id="094e0-403">To resolve this issue, add Hyper-V modules to initramfs and rebuild it:</span></span>

<span data-ttu-id="094e0-404">Upravit `/etc/dracut.conf`a přidejte následující obsah:</span><span class="sxs-lookup"><span data-stu-id="094e0-404">Edit `/etc/dracut.conf`, and add the following content:</span></span>

        add_drivers+="hv_vmbus hv_netvsc hv_storvsc"

<span data-ttu-id="094e0-405">Znovu sestavte initramfs:</span><span class="sxs-lookup"><span data-stu-id="094e0-405">Rebuild initramfs:</span></span>

        # dracut -f -v

<span data-ttu-id="094e0-406">Další podrobnosti najdete v tématu informace [znovu sestavit initramfs](https://access.redhat.com/solutions/1958).</span><span class="sxs-lookup"><span data-stu-id="094e0-406">For more details, see the information about [rebuilding initramfs](https://access.redhat.com/solutions/1958).</span></span>

## <a name="next-steps"></a><span data-ttu-id="094e0-407">Další kroky</span><span class="sxs-lookup"><span data-stu-id="094e0-407">Next steps</span></span>
<span data-ttu-id="094e0-408">Nyní jste připraveni používat virtuální pevný disk Red Hat Enterprise Linux k vytvoření nové virtuální počítače v Azure.</span><span class="sxs-lookup"><span data-stu-id="094e0-408">You're now ready to use your Red Hat Enterprise Linux virtual hard disk to create new virtual machines in Azure.</span></span> <span data-ttu-id="094e0-409">Pokud je poprvé, že jste nahrávání souboru VHD do Azure, najdete v části kroky 2 a 3 v [vytváření a odesílání virtuální pevný disk, který obsahuje operační systém Linux](classic/create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="094e0-409">If this is the first time that you're uploading the .vhd file to Azure, see steps 2 and 3 in [Creating and uploading a virtual hard disk that contains the Linux operating system](classic/create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).</span></span>

<span data-ttu-id="094e0-410">Další podrobnosti o hypervisorů, které jsou certifikované ke spuštění Red Hat Enterprise Linux najdete v tématu [webu Red Hat](https://access.redhat.com/certified-hypervisors).</span><span class="sxs-lookup"><span data-stu-id="094e0-410">For more details about the hypervisors that are certified to run Red Hat Enterprise Linux, see [the Red Hat website](https://access.redhat.com/certified-hypervisors).</span></span>
