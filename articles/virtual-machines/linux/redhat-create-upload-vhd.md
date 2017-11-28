---
title: "aaaCreate a nahrát VHD Red Hat Enterprise Linux pro použití v Azure | Microsoft Docs"
description: "Další informace toocreate a nahrajte Azure virtuální pevný disk (VHD) obsahující operační systém Red Hat Linux."
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
ms.openlocfilehash: bdd1bbfbee55b5cc61d69a09b06b6bd2c3de362b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="prepare-a-red-hat-based-virtual-machine-for-azure"></a><span data-ttu-id="f30eb-103">Příprava virtuálního počítače založeného na Red Hat pro Azure</span><span class="sxs-lookup"><span data-stu-id="f30eb-103">Prepare a Red Hat-based virtual machine for Azure</span></span>
<span data-ttu-id="f30eb-104">V tomto článku se dozvíte, jak tooprepare virtuální počítač Red Hat Enterprise Linux (RHEL) pro použití v Azure.</span><span class="sxs-lookup"><span data-stu-id="f30eb-104">In this article, you will learn how tooprepare a Red Hat Enterprise Linux (RHEL) virtual machine for use in Azure.</span></span> <span data-ttu-id="f30eb-105">Hello verze RHEL, které jsou popsané v tomto článku jsou 6.7 + a 7.1 +.</span><span class="sxs-lookup"><span data-stu-id="f30eb-105">hello versions of RHEL that are covered in this article are 6.7+ and 7.1+.</span></span> <span data-ttu-id="f30eb-106">Hello hypervisory pro přípravu, které jsou popsané v tomto článku jsou virtuální počítače Hyper-V, na základě jádra (KVM) a VMware.</span><span class="sxs-lookup"><span data-stu-id="f30eb-106">hello hypervisors for preparation that are covered in this article are Hyper-V, kernel-based virtual machine (KVM), and VMware.</span></span> <span data-ttu-id="f30eb-107">Další informace o požadavcích na podmínky pro účasti v programu Red Hat přístup ke cloudu najdete v tématu [webu přístup do cloudu Red Hat](http://www.redhat.com/en/technologies/cloud-computing/cloud-access) a [systémem RHEL v Azure](https://access.redhat.com/ecosystem/ccsp/microsoft-azure).</span><span class="sxs-lookup"><span data-stu-id="f30eb-107">For more information about eligibility requirements for participating in Red Hat's Cloud Access program, see [Red Hat's Cloud Access website](http://www.redhat.com/en/technologies/cloud-computing/cloud-access) and [Running RHEL on Azure](https://access.redhat.com/ecosystem/ccsp/microsoft-azure).</span></span>

## <a name="prepare-a-red-hat-based-virtual-machine-from-hyper-v-manager"></a><span data-ttu-id="f30eb-108">Příprava virtuálního počítače, na základě Red Hat ze Správce technologie Hyper-V</span><span class="sxs-lookup"><span data-stu-id="f30eb-108">Prepare a Red Hat-based virtual machine from Hyper-V Manager</span></span>

### <a name="prerequisites"></a><span data-ttu-id="f30eb-109">Požadavky</span><span class="sxs-lookup"><span data-stu-id="f30eb-109">Prerequisites</span></span>
<span data-ttu-id="f30eb-110">V této části se předpokládá, že jste již získali soubor ISO od hello Red Hat webu a nainstalované hello RHEL image tooa virtuální pevný disk (VHD).</span><span class="sxs-lookup"><span data-stu-id="f30eb-110">This section assumes that you have already obtained an ISO file from hello Red Hat website and installed hello RHEL image tooa virtual hard disk (VHD).</span></span> <span data-ttu-id="f30eb-111">Další informace o toouse Správce technologie Hyper-V tooinstall image operačního systému najdete v části [instalace hello Role Hyper-V a konfigurace virtuálního počítače](http://technet.microsoft.com/library/hh846766.aspx).</span><span class="sxs-lookup"><span data-stu-id="f30eb-111">For more details about how toouse Hyper-V Manager tooinstall an operating system image, see [Install hello Hyper-V Role and Configure a Virtual Machine](http://technet.microsoft.com/library/hh846766.aspx).</span></span>

<span data-ttu-id="f30eb-112">**Poznámky k instalaci RHEL**</span><span class="sxs-lookup"><span data-stu-id="f30eb-112">**RHEL installation notes**</span></span>

* <span data-ttu-id="f30eb-113">Azure nepodporuje hello formátu VHDX.</span><span class="sxs-lookup"><span data-stu-id="f30eb-113">Azure does not support hello VHDX format.</span></span> <span data-ttu-id="f30eb-114">Azure podporuje pouze dlouhodobý virtuálního pevného disku.</span><span class="sxs-lookup"><span data-stu-id="f30eb-114">Azure supports only fixed VHD.</span></span> <span data-ttu-id="f30eb-115">Můžete použít formát tooVHD disku hello tooconvert Správce technologie Hyper-V, nebo můžete pomocí rutiny convert-VHD prostředí hello.</span><span class="sxs-lookup"><span data-stu-id="f30eb-115">You can use Hyper-V Manager tooconvert hello disk tooVHD format, or you can use hello convert-vhd cmdlet.</span></span> <span data-ttu-id="f30eb-116">Pokud používáte VirtualBox, vyberte **pevnou velikost** názvem na rozdíl od toohello výchozí možnost přidělí dynamicky při vytvoření disku hello.</span><span class="sxs-lookup"><span data-stu-id="f30eb-116">If you use VirtualBox, select **Fixed size** as opposed toohello default dynamically allocated option when you create hello disk.</span></span>
* <span data-ttu-id="f30eb-117">Azure podporuje pouze virtuální počítače generace 1.</span><span class="sxs-lookup"><span data-stu-id="f30eb-117">Azure supports only generation 1 virtual machines.</span></span> <span data-ttu-id="f30eb-118">Virtuální počítač generace 1 můžete převést z formátu souboru virtuálního pevného disku VHDX toohello a dynamicky se zvětšující tooa disk pevné velikosti.</span><span class="sxs-lookup"><span data-stu-id="f30eb-118">You can convert a generation 1 virtual machine from VHDX toohello VHD file format and from dynamically expanding tooa fixed-size disk.</span></span> <span data-ttu-id="f30eb-119">Nelze změnit generaci virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="f30eb-119">You can't change a virtual machine's generation.</span></span> <span data-ttu-id="f30eb-120">Další informace najdete v tématu [by měl vytvořit virtuální počítač generace 1 nebo 2 v Hyper-V?](https://technet.microsoft.com/windows-server-docs/compute/hyper-v/plan/should-i-create-a-generation-1-or-2-virtual-machine-in-hyper-v).</span><span class="sxs-lookup"><span data-stu-id="f30eb-120">For more information, see [Should I create a generation 1 or 2 virtual machine in Hyper-V?](https://technet.microsoft.com/windows-server-docs/compute/hyper-v/plan/should-i-create-a-generation-1-or-2-virtual-machine-in-hyper-v).</span></span>
* <span data-ttu-id="f30eb-121">Hello maximální povolenou velikost pro hello virtuálního pevného disku je 1,023 GB.</span><span class="sxs-lookup"><span data-stu-id="f30eb-121">hello maximum size that's allowed for hello VHD is 1,023 GB.</span></span>
* <span data-ttu-id="f30eb-122">Při instalaci operačního systému Linux hello, doporučujeme použít standardní oddíly spíše než logické svazku Manager (LVM), což je často hello výchozí pro mnoho instalace.</span><span class="sxs-lookup"><span data-stu-id="f30eb-122">When you install hello Linux operating system, we recommend that you use standard partitions rather than Logical Volume Manager (LVM), which is often hello default for many installations.</span></span> <span data-ttu-id="f30eb-123">Tento postup zabráníte LVM název je v konfliktu s klonovaný virtuální počítače, zvlášť pokud byste někdy potřebovali tooattach operačního systému disku tooanother identický virtuální počítač pro řešení potíží.</span><span class="sxs-lookup"><span data-stu-id="f30eb-123">This practice will avoid LVM name conflicts with cloned virtual machines, particularly if you ever need tooattach an operating system disk tooanother identical virtual machine for troubleshooting.</span></span> <span data-ttu-id="f30eb-124">[LVM](configure-lvm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) nebo [RAID](configure-raid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) mohou být použity na datové disky.</span><span class="sxs-lookup"><span data-stu-id="f30eb-124">[LVM](configure-lvm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) or [RAID](configure-raid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) may be used on data disks.</span></span>
* <span data-ttu-id="f30eb-125">Vyžaduje se podpora jádra pro připojení systémy souborů univerzální formát disku (UDF).</span><span class="sxs-lookup"><span data-stu-id="f30eb-125">Kernel support for mounting Universal Disk Format (UDF) file systems is required.</span></span> <span data-ttu-id="f30eb-126">Při prvním spuštění v Azure, hello formátu UDF médiu, které se předá připojené toohello hosta hello zřizování konfigurace toohello Linux virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="f30eb-126">At first boot on Azure, hello UDF-formatted media that is attached toohello guest passes hello provisioning configuration toohello Linux virtual machine.</span></span> <span data-ttu-id="f30eb-127">Hello Azure Linux Agent musí být schopný toomount hello UDF soubor systému tooread své konfiguraci a zřízení hello virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="f30eb-127">hello Azure Linux Agent must be able toomount hello UDF file system tooread its configuration and provision hello virtual machine.</span></span>
* <span data-ttu-id="f30eb-128">Verze hello Linux jádra, které jsou starší než 2.6.37 nepodporují přístup k nerovnoměrné paměti (NUMA) s technologií Hyper-V s větší velikostí virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="f30eb-128">Versions of hello Linux kernel that are earlier than 2.6.37 do not support non-uniform memory access (NUMA) on Hyper-V with larger virtual machine sizes.</span></span> <span data-ttu-id="f30eb-129">-Li tento problém ovlivňuje především starší distribuce, které používají hello proti proudu Red Hat 2.6.32 jádra která byla opravena v RHEL 6.6 (jádra 2.6.32 504).</span><span class="sxs-lookup"><span data-stu-id="f30eb-129">This issue primarily impacts older distributions that use hello upstream Red Hat 2.6.32 kernel and was fixed in RHEL 6.6 (kernel-2.6.32-504).</span></span> <span data-ttu-id="f30eb-130">Systémy, které spouštějí vlastní jádra, které jsou starší než 2.6.37 nebo na základě RHEL jádra, která jsou starší než 2.6.32-504 musíte nastavit hello `numa=off` spouštění na příkazovém řádku jádra hello v grub.conf parametr.</span><span class="sxs-lookup"><span data-stu-id="f30eb-130">Systems that run custom kernels that are older than 2.6.37 or RHEL-based kernels that are older than 2.6.32-504 must set hello `numa=off` boot parameter on hello kernel command line in grub.conf.</span></span> <span data-ttu-id="f30eb-131">Další informace najdete v tématu Red Hat [KB 436883](https://access.redhat.com/solutions/436883).</span><span class="sxs-lookup"><span data-stu-id="f30eb-131">For more information, see Red Hat [KB 436883](https://access.redhat.com/solutions/436883).</span></span>
* <span data-ttu-id="f30eb-132">Nekonfigurujte přepnutí oddílu na disku operačního systému hello.</span><span class="sxs-lookup"><span data-stu-id="f30eb-132">Do not configure a swap partition on hello operating system disk.</span></span> <span data-ttu-id="f30eb-133">Hello Linux Agent může být nakonfigurované toocreate odkládací soubor na disku hello dočasných prostředků.</span><span class="sxs-lookup"><span data-stu-id="f30eb-133">hello Linux Agent can be configured toocreate a swap file on hello temporary resource disk.</span></span>  <span data-ttu-id="f30eb-134">Další informace o této naleznete v hello následující kroky.</span><span class="sxs-lookup"><span data-stu-id="f30eb-134">More information about this can be found in hello following steps.</span></span>
* <span data-ttu-id="f30eb-135">Všechny virtuální pevné disky musí mít velikostí, které jsou násobky 1 MB.</span><span class="sxs-lookup"><span data-stu-id="f30eb-135">All VHDs must have sizes that are multiples of 1 MB.</span></span>

### <a name="prepare-a-rhel-6-virtual-machine-from-hyper-v-manager"></a><span data-ttu-id="f30eb-136">Příprava systému RHEL 6 virtuálního počítače ze Správce technologie Hyper-V</span><span class="sxs-lookup"><span data-stu-id="f30eb-136">Prepare a RHEL 6 virtual machine from Hyper-V Manager</span></span>

1. <span data-ttu-id="f30eb-137">Ve Správci technologie Hyper-V vyberte hello virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="f30eb-137">In Hyper-V Manager, select hello virtual machine.</span></span>

2. <span data-ttu-id="f30eb-138">Klikněte na tlačítko **připojit** tooopen okna konzoly pro virtuální počítač hello.</span><span class="sxs-lookup"><span data-stu-id="f30eb-138">Click **Connect** tooopen a console window for hello virtual machine.</span></span>

3. <span data-ttu-id="f30eb-139">V systému RHEL 6 NetworkManager může narušovat hello Azure Linux agent.</span><span class="sxs-lookup"><span data-stu-id="f30eb-139">In RHEL 6, NetworkManager can interfere with hello Azure Linux agent.</span></span> <span data-ttu-id="f30eb-140">Odinstalaci tohoto balíčku spuštěním hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="f30eb-140">Uninstall this package by running hello following command:</span></span>
   
        # sudo rpm -e --nodeps NetworkManager

4. <span data-ttu-id="f30eb-141">Vytvořte nebo upravte hello `/etc/sysconfig/network` souboru a přidejte hello následující text:</span><span class="sxs-lookup"><span data-stu-id="f30eb-141">Create or edit hello `/etc/sysconfig/network` file, and add hello following text:</span></span>
   
        NETWORKING=yes
        HOSTNAME=localhost.localdomain

5. <span data-ttu-id="f30eb-142">Vytvořte nebo upravte hello `/etc/sysconfig/network-scripts/ifcfg-eth0` souboru a přidejte hello následující text:</span><span class="sxs-lookup"><span data-stu-id="f30eb-142">Create or edit hello `/etc/sysconfig/network-scripts/ifcfg-eth0` file, and add hello following text:</span></span>
   
        DEVICE=eth0
        ONBOOT=yes
        BOOTPROTO=dhcp
        TYPE=Ethernet
        USERCTL=no
        PEERDNS=yes
        IPV6INIT=no

6. <span data-ttu-id="f30eb-143">(Nebo odebrat) hello udev pravidla tooavoid generování statická pravidla pro rozhraní sítě Ethernet hello.</span><span class="sxs-lookup"><span data-stu-id="f30eb-143">Move (or remove) hello udev rules tooavoid generating static rules for hello Ethernet interface.</span></span> <span data-ttu-id="f30eb-144">Tato pravidla způsobit problémy při klonování virtuálního počítače v Microsoft Azure nebo Hyper-V:</span><span class="sxs-lookup"><span data-stu-id="f30eb-144">These rules cause problems when you clone a virtual machine in Microsoft Azure or Hyper-V:</span></span>

        # sudo ln -s /dev/null /etc/udev/rules.d/75-persistent-net-generator.rules
        
        # sudo rm -f /etc/udev/rules.d/70-persistent-net.rules

7. <span data-ttu-id="f30eb-145">Ujistěte se, že hello síťová služba se spustí při spuštění spuštěním hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="f30eb-145">Ensure that hello network service will start at boot time by running hello following command:</span></span>

        # sudo chkconfig network on

8. <span data-ttu-id="f30eb-146">Zaregistrujte předplatné Red Hat tooenable hello instalace balíčků z úložiště RHEL hello spuštěním hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="f30eb-146">Register your Red Hat subscription tooenable hello installation of packages from hello RHEL repository by running hello following command:</span></span>

        # sudo subscription-manager register --auto-attach --username=XXX --password=XXX

9. <span data-ttu-id="f30eb-147">balíček WALinuxAgent Hello `WALinuxAgent-<version>`, bylo posunuto toohello Red Hat funkce úložiště.</span><span class="sxs-lookup"><span data-stu-id="f30eb-147">hello WALinuxAgent package, `WALinuxAgent-<version>`, has been pushed toohello Red Hat extras repository.</span></span> <span data-ttu-id="f30eb-148">Povolení funkce úložiště hello spuštěním hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="f30eb-148">Enable hello extras repository by running hello following command:</span></span>

        # subscription-manager repos --enable=rhel-6-server-extras-rpms

10. <span data-ttu-id="f30eb-149">Upravte hello jádra spouštěcí řádku grub konfigurace tooinclude další jádra parametry pro Azure.</span><span class="sxs-lookup"><span data-stu-id="f30eb-149">Modify hello kernel boot line in your grub configuration tooinclude additional kernel parameters for Azure.</span></span> <span data-ttu-id="f30eb-150">toodo tato úprava, otevřete `/boot/grub/menu.lst` v textovém editoru a ujistěte se, že jádra výchozí hello zahrnuje hello následující parametry:</span><span class="sxs-lookup"><span data-stu-id="f30eb-150">toodo this modification, open `/boot/grub/menu.lst` in a text editor, and ensure that hello default kernel includes hello following parameters:</span></span>
    
        console=ttyS0 earlyprintk=ttyS0 rootdelay=300
    
    <span data-ttu-id="f30eb-151">To také zajistí, že všechny zprávy konzoly jsou odesílány toohello první sériového portu, který může být užitečné Azure podporu pro ladění problémů.</span><span class="sxs-lookup"><span data-stu-id="f30eb-151">This will also ensure that all console messages are sent toohello first serial port, which can assist Azure support with debugging issues.</span></span>
    
    <span data-ttu-id="f30eb-152">Kromě toho doporučujeme odebrat hello následující parametry:</span><span class="sxs-lookup"><span data-stu-id="f30eb-152">In addition, we recommended that you remove hello following parameters:</span></span>
    
        rhgb quiet crashkernel=auto
    
    <span data-ttu-id="f30eb-153">Grafické a quiet spouštěcí nejsou užitečné při cloudovém prostředí, kde chceme, aby všechny protokoly toobe hello odeslané toohello sériového portu.</span><span class="sxs-lookup"><span data-stu-id="f30eb-153">Graphical and quiet boot are not useful in a cloud environment where we want all hello logs toobe sent toohello serial port.</span></span>  <span data-ttu-id="f30eb-154">Můžete ponechat hello `crashkernel` možnost nakonfigurované v případě potřeby.</span><span class="sxs-lookup"><span data-stu-id="f30eb-154">You can leave hello `crashkernel` option configured if desired.</span></span> <span data-ttu-id="f30eb-155">Všimněte si, že tento parametr snižuje hello množství dostupné paměti ve virtuálním počítači hello 128 MB a víc.</span><span class="sxs-lookup"><span data-stu-id="f30eb-155">Note that this parameter reduces hello amount of available memory in hello virtual machine by 128 MB or more.</span></span> <span data-ttu-id="f30eb-156">Tato konfigurace může způsobovat problémy menší velikostí virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="f30eb-156">This configuration might be problematic on smaller virtual machine sizes.</span></span>

    >[!Important]
    <span data-ttu-id="f30eb-157">RHEL verze 6.5 a starší, musíte taky nastavit hello `numa=off` jádra parametr.</span><span class="sxs-lookup"><span data-stu-id="f30eb-157">RHEL 6.5 and earlier must also set hello `numa=off` kernel parameter.</span></span> <span data-ttu-id="f30eb-158">V tématu Red Hat [KB 436883](https://access.redhat.com/solutions/436883).</span><span class="sxs-lookup"><span data-stu-id="f30eb-158">See Red Hat [KB 436883](https://access.redhat.com/solutions/436883).</span></span>

11. <span data-ttu-id="f30eb-159">Ujistěte se, tento server hello zabezpečené shell (SSH) je nainstalována a nakonfigurována toostart při spuštění, který je obvykle výchozí hello.</span><span class="sxs-lookup"><span data-stu-id="f30eb-159">Ensure that hello secure shell (SSH) server is installed and configured toostart at boot time, which is usually hello default.</span></span> <span data-ttu-id="f30eb-160">Upravte hello tooinclude /etc/ssh/sshd_config následující řádek:</span><span class="sxs-lookup"><span data-stu-id="f30eb-160">Modify /etc/ssh/sshd_config tooinclude hello following line:</span></span>

        ClientAliveInterval 180

12. <span data-ttu-id="f30eb-161">Nainstalujte hello Azure Linux Agent spuštěním hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="f30eb-161">Install hello Azure Linux Agent by running hello following command:</span></span>

        # sudo yum install WALinuxAgent

        # sudo chkconfig waagent on

    <span data-ttu-id="f30eb-162">Instalaci balíčku WALinuxAgent hello odebere hello NetworkManager a NetworkManager gnome balíčky, pokud nebyly již odebrány v kroku 3.</span><span class="sxs-lookup"><span data-stu-id="f30eb-162">Installing hello WALinuxAgent package removes hello NetworkManager and NetworkManager-gnome packages if they were not already removed in step 3.</span></span>

13. <span data-ttu-id="f30eb-163">Nevytvářejte odkládacího prostoru na disku operačního systému hello.</span><span class="sxs-lookup"><span data-stu-id="f30eb-163">Do not create swap space on hello operating system disk.</span></span>

    <span data-ttu-id="f30eb-164">Hello Azure Linux Agent mohou automaticky konfigurovat odkládacího souboru pomocí disku hello místní prostředek, který je připojený toohello virtuální počítač po zřízení hello virtuálního počítače na platformě Azure.</span><span class="sxs-lookup"><span data-stu-id="f30eb-164">hello Azure Linux Agent can automatically configure swap space by using hello local resource disk that is attached toohello virtual machine after hello virtual machine is provisioned on Azure.</span></span> <span data-ttu-id="f30eb-165">Všimněte si, že hello místní disk prostředků je dočasný a že ji může být vyprázdněna při hello virtuálního počítače je zrušit.</span><span class="sxs-lookup"><span data-stu-id="f30eb-165">Note that hello local resource disk is a temporary disk and that it might be emptied when hello virtual machine is deprovisioned.</span></span> <span data-ttu-id="f30eb-166">Po instalaci hello Azure Linux Agent hello předchozího kroku, upravte hello následující parametry v /etc/waagent.conf odpovídajícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="f30eb-166">After you install hello Azure Linux Agent in hello previous step, modify hello following parameters in /etc/waagent.conf appropriately:</span></span>

        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this toowhatever you need it toobe.

14. <span data-ttu-id="f30eb-167">Zrušení registrace předplatného hello (v případě potřeby) tak, že spustíte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="f30eb-167">Unregister hello subscription (if necessary) by running hello following command:</span></span>

        # sudo subscription-manager unregister

15. <span data-ttu-id="f30eb-168">Spusťte hello následující příkazy toodeprovision hello virtuálního počítače a jeho přípravu pro zřizování na Azure:</span><span class="sxs-lookup"><span data-stu-id="f30eb-168">Run hello following commands toodeprovision hello virtual machine and prepare it for provisioning on Azure:</span></span>

        # sudo waagent -force -deprovision

        # export HISTSIZE=0

        # logout

16. <span data-ttu-id="f30eb-169">Klikněte na tlačítko **akce** > **vypnout** ve Správci technologie Hyper-V.</span><span class="sxs-lookup"><span data-stu-id="f30eb-169">Click **Action** > **Shut Down** in Hyper-V Manager.</span></span> <span data-ttu-id="f30eb-170">Svůj disk VHD Linux je nyní připraven toobe nahrán tooAzure.</span><span class="sxs-lookup"><span data-stu-id="f30eb-170">Your Linux VHD is now ready toobe uploaded tooAzure.</span></span>


### <a name="prepare-a-rhel-7-virtual-machine-from-hyper-v-manager"></a><span data-ttu-id="f30eb-171">Příprava RHEL 7 virtuálního počítače ze Správce technologie Hyper-V</span><span class="sxs-lookup"><span data-stu-id="f30eb-171">Prepare a RHEL 7 virtual machine from Hyper-V Manager</span></span>

1. <span data-ttu-id="f30eb-172">Ve Správci technologie Hyper-V vyberte hello virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="f30eb-172">In Hyper-V Manager, select hello virtual machine.</span></span>

2. <span data-ttu-id="f30eb-173">Klikněte na tlačítko **připojit** tooopen okna konzoly pro virtuální počítač hello.</span><span class="sxs-lookup"><span data-stu-id="f30eb-173">Click **Connect** tooopen a console window for hello virtual machine.</span></span>

3. <span data-ttu-id="f30eb-174">Vytvořte nebo upravte hello `/etc/sysconfig/network` souboru a přidejte hello následující text:</span><span class="sxs-lookup"><span data-stu-id="f30eb-174">Create or edit hello `/etc/sysconfig/network` file, and add hello following text:</span></span>
   
        NETWORKING=yes
        HOSTNAME=localhost.localdomain

4. <span data-ttu-id="f30eb-175">Vytvořte nebo upravte hello `/etc/sysconfig/network-scripts/ifcfg-eth0` souboru a přidejte hello následující text:</span><span class="sxs-lookup"><span data-stu-id="f30eb-175">Create or edit hello `/etc/sysconfig/network-scripts/ifcfg-eth0` file, and add hello following text:</span></span>
   
        DEVICE=eth0
        ONBOOT=yes
        BOOTPROTO=dhcp
        TYPE=Ethernet
        USERCTL=no
        PEERDNS=yes
        IPV6INIT=no
        NM_CONTROLLED=no

5. <span data-ttu-id="f30eb-176">Ujistěte se, že hello síťová služba se spustí při spuštění spuštěním hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="f30eb-176">Ensure that hello network service will start at boot time by running hello following command:</span></span>

        # sudo chkconfig network on

6. <span data-ttu-id="f30eb-177">Zaregistrujte předplatné Red Hat tooenable hello instalace balíčků z úložiště RHEL hello spuštěním hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="f30eb-177">Register your Red Hat subscription tooenable hello installation of packages from hello RHEL repository by running hello following command:</span></span>

        # sudo subscription-manager register --auto-attach --username=XXX --password=XXX

7. <span data-ttu-id="f30eb-178">Upravte hello jádra spouštěcí řádku grub konfigurace tooinclude další jádra parametry pro Azure.</span><span class="sxs-lookup"><span data-stu-id="f30eb-178">Modify hello kernel boot line in your grub configuration tooinclude additional kernel parameters for Azure.</span></span> <span data-ttu-id="f30eb-179">toodo tato úprava, otevřete `/etc/default/grub` v textovém editoru a upravit hello `GRUB_CMDLINE_LINUX` parametr.</span><span class="sxs-lookup"><span data-stu-id="f30eb-179">toodo this modification, open `/etc/default/grub` in a text editor, and edit hello `GRUB_CMDLINE_LINUX` parameter.</span></span> <span data-ttu-id="f30eb-180">Například:</span><span class="sxs-lookup"><span data-stu-id="f30eb-180">For example:</span></span>
   
        GRUB_CMDLINE_LINUX="rootdelay=300 console=ttyS0 earlyprintk=ttyS0 net.ifnames=0"
   
   <span data-ttu-id="f30eb-181">To také zajistí, že všechny zprávy konzoly jsou odesílány toohello první sériového portu, který může být užitečné Azure podporu pro ladění problémů.</span><span class="sxs-lookup"><span data-stu-id="f30eb-181">This will also ensure that all console messages are sent toohello first serial port, which can assist Azure support with debugging issues.</span></span> <span data-ttu-id="f30eb-182">Tato konfigurace taky vypne hello nové RHEL 7 zásady vytváření názvů pro síťové adaptéry.</span><span class="sxs-lookup"><span data-stu-id="f30eb-182">This configuration also turns off hello new RHEL 7 naming conventions for NICs.</span></span> <span data-ttu-id="f30eb-183">Kromě toho doporučujeme odebrat hello následující parametry:</span><span class="sxs-lookup"><span data-stu-id="f30eb-183">In addition, we recommend that you remove hello following parameters:</span></span>
   
        rhgb quiet crashkernel=auto
   
    <span data-ttu-id="f30eb-184">Grafické a quiet spouštěcí nejsou užitečné při cloudovém prostředí, kde chceme, aby všechny protokoly toobe hello odeslané toohello sériového portu.</span><span class="sxs-lookup"><span data-stu-id="f30eb-184">Graphical and quiet boot are not useful in a cloud environment where we want all hello logs toobe sent toohello serial port.</span></span> <span data-ttu-id="f30eb-185">Můžete ponechat hello `crashkernel` možnost nakonfigurované v případě potřeby.</span><span class="sxs-lookup"><span data-stu-id="f30eb-185">You can leave hello `crashkernel` option configured if desired.</span></span> <span data-ttu-id="f30eb-186">Všimněte si, že tento parametr snižuje hello množství dostupné paměti v hello virtuálního počítače pomocí 128 MB nebo víc, což může způsobovat problémy menší velikostí virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="f30eb-186">Note that this parameter reduces hello amount of available memory in hello virtual machine by 128 MB or more, which might be problematic on smaller virtual machine sizes.</span></span>

8. <span data-ttu-id="f30eb-187">Po dokončení úprav `/etc/default/grub`spusťte hello následující příkaz toorebuild hello grub konfigurace:</span><span class="sxs-lookup"><span data-stu-id="f30eb-187">After you are done editing `/etc/default/grub`, run hello following command toorebuild hello grub configuration:</span></span>

        # sudo grub2-mkconfig -o /boot/grub2/grub.cfg

9. <span data-ttu-id="f30eb-188">Ujistěte se, že hello SSH server je nainstalován a nakonfigurován toostart při spuštění, který je obvykle výchozí hello.</span><span class="sxs-lookup"><span data-stu-id="f30eb-188">Ensure that hello SSH server is installed and configured toostart at boot time, which is usually hello default.</span></span> <span data-ttu-id="f30eb-189">Upravit `/etc/ssh/sshd_config` tooinclude hello následující řádek:</span><span class="sxs-lookup"><span data-stu-id="f30eb-189">Modify `/etc/ssh/sshd_config` tooinclude hello following line:</span></span>

        ClientAliveInterval 180

10. <span data-ttu-id="f30eb-190">balíček WALinuxAgent Hello `WALinuxAgent-<version>`, bylo posunuto toohello Red Hat funkce úložiště.</span><span class="sxs-lookup"><span data-stu-id="f30eb-190">hello WALinuxAgent package, `WALinuxAgent-<version>`, has been pushed toohello Red Hat extras repository.</span></span> <span data-ttu-id="f30eb-191">Povolení funkce úložiště hello spuštěním hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="f30eb-191">Enable hello extras repository by running hello following command:</span></span>

        # subscription-manager repos --enable=rhel-7-server-extras-rpms

11. <span data-ttu-id="f30eb-192">Nainstalujte hello Azure Linux Agent spuštěním hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="f30eb-192">Install hello Azure Linux Agent by running hello following command:</span></span>

        # sudo yum install WALinuxAgent

        # sudo systemctl enable waagent.service

12. <span data-ttu-id="f30eb-193">Nevytvářejte odkládacího prostoru na disku operačního systému hello.</span><span class="sxs-lookup"><span data-stu-id="f30eb-193">Do not create swap space on hello operating system disk.</span></span>

    <span data-ttu-id="f30eb-194">Hello Azure Linux Agent mohou automaticky konfigurovat odkládacího souboru pomocí disku hello místní prostředek, který je připojený toohello virtuální počítač po zřízení hello virtuálního počítače na platformě Azure.</span><span class="sxs-lookup"><span data-stu-id="f30eb-194">hello Azure Linux Agent can automatically configure swap space by using hello local resource disk that is attached toohello virtual machine after hello virtual machine is provisioned on Azure.</span></span> <span data-ttu-id="f30eb-195">Upozorňujeme, že hello místní prostředek disk je dočasný a může být vyprázdnit po deprovisioned hello virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="f30eb-195">Note that hello local resource disk is a temporary disk, and it might be emptied when hello virtual machine is deprovisioned.</span></span> <span data-ttu-id="f30eb-196">Po instalaci hello Azure Linux Agent v předchozím kroku hello se upravit hello následující parametry v `/etc/waagent.conf` odpovídajícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="f30eb-196">After you install hello Azure Linux Agent in hello previous step, modify hello following parameters in `/etc/waagent.conf` appropriately:</span></span>

        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this toowhatever you need it toobe.

13. <span data-ttu-id="f30eb-197">Pokud chcete toounregister hello předplatné, spusťte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="f30eb-197">If you want toounregister hello subscription, run hello following command:</span></span>

        # sudo subscription-manager unregister

14. <span data-ttu-id="f30eb-198">Spusťte hello následující příkazy toodeprovision hello virtuálního počítače a jeho přípravu pro zřizování na Azure:</span><span class="sxs-lookup"><span data-stu-id="f30eb-198">Run hello following commands toodeprovision hello virtual machine and prepare it for provisioning on Azure:</span></span>

        # sudo waagent -force -deprovision

        # export HISTSIZE=0

        # logout

15. <span data-ttu-id="f30eb-199">Klikněte na tlačítko **akce** > **vypnout** ve Správci technologie Hyper-V.</span><span class="sxs-lookup"><span data-stu-id="f30eb-199">Click **Action** > **Shut Down** in Hyper-V Manager.</span></span> <span data-ttu-id="f30eb-200">Svůj disk VHD Linux je nyní připraven toobe nahrán tooAzure.</span><span class="sxs-lookup"><span data-stu-id="f30eb-200">Your Linux VHD is now ready toobe uploaded tooAzure.</span></span>


## <a name="prepare-a-red-hat-based-virtual-machine-from-kvm"></a><span data-ttu-id="f30eb-201">Příprava virtuálního počítače, na základě Red Hat z KVM</span><span class="sxs-lookup"><span data-stu-id="f30eb-201">Prepare a Red Hat-based virtual machine from KVM</span></span>
### <a name="prepare-a-rhel-6-virtual-machine-from-kvm"></a><span data-ttu-id="f30eb-202">Příprava systému RHEL 6 virtuálního počítače z KVM</span><span class="sxs-lookup"><span data-stu-id="f30eb-202">Prepare a RHEL 6 virtual machine from KVM</span></span>

1. <span data-ttu-id="f30eb-203">Stáhněte z webu Red Hat hello hello KVM bitovou kopii systému RHEL 6.</span><span class="sxs-lookup"><span data-stu-id="f30eb-203">Download hello KVM image of RHEL 6 from hello Red Hat website.</span></span>

2. <span data-ttu-id="f30eb-204">Nastavení kořenové heslo.</span><span class="sxs-lookup"><span data-stu-id="f30eb-204">Set a root password.</span></span>

    <span data-ttu-id="f30eb-205">Generovat zašifrované heslo a zkopírujte hello výstup hello příkazu:</span><span class="sxs-lookup"><span data-stu-id="f30eb-205">Generate an encrypted password, and copy hello output of hello command:</span></span>

        # openssl passwd -1 changeme

    <span data-ttu-id="f30eb-206">Nastavení kořenové heslo s guestfish:</span><span class="sxs-lookup"><span data-stu-id="f30eb-206">Set a root password with guestfish:</span></span>
        
        # guestfish --rw -a <image-name>
        > <fs> run
        > <fs> list-filesystems
        > <fs> mount /dev/sda1 /
        > <fs> vi /etc/shadow
        > <fs> exit

   <span data-ttu-id="f30eb-207">Změna hello druhé pole hello kořenového uživatele z "!"</span><span class="sxs-lookup"><span data-stu-id="f30eb-207">Change hello second field of hello root user from "!!"</span></span> <span data-ttu-id="f30eb-208">toohello zašifrované heslo.</span><span class="sxs-lookup"><span data-stu-id="f30eb-208">toohello encrypted password.</span></span>

3. <span data-ttu-id="f30eb-209">Vytvoření virtuálního počítače v KVM z hello qcow2 image.</span><span class="sxs-lookup"><span data-stu-id="f30eb-209">Create a virtual machine in KVM from hello qcow2 image.</span></span> <span data-ttu-id="f30eb-210">Nastavte typ disku hello příliš**qcow2**a nastavte model zařízení rozhraní virtuální sítě hello příliš**virtio**.</span><span class="sxs-lookup"><span data-stu-id="f30eb-210">Set hello disk type too**qcow2**, and set hello virtual network interface device model too**virtio**.</span></span> <span data-ttu-id="f30eb-211">Potom spusťte hello virtuálního počítače a přihlaste se jako kořenový.</span><span class="sxs-lookup"><span data-stu-id="f30eb-211">Then, start hello virtual machine, and sign in as root.</span></span>

4. <span data-ttu-id="f30eb-212">Vytvořte nebo upravte hello `/etc/sysconfig/network` souboru a přidejte hello následující text:</span><span class="sxs-lookup"><span data-stu-id="f30eb-212">Create or edit hello `/etc/sysconfig/network` file, and add hello following text:</span></span>
   
        NETWORKING=yes
        HOSTNAME=localhost.localdomain

5. <span data-ttu-id="f30eb-213">Vytvořte nebo upravte hello `/etc/sysconfig/network-scripts/ifcfg-eth0` souboru a přidejte hello následující text:</span><span class="sxs-lookup"><span data-stu-id="f30eb-213">Create or edit hello `/etc/sysconfig/network-scripts/ifcfg-eth0` file, and add hello following text:</span></span>
   
        DEVICE=eth0
        ONBOOT=yes
        BOOTPROTO=dhcp
        TYPE=Ethernet
        USERCTL=no
        PEERDNS=yes
        IPV6INIT=no

6. <span data-ttu-id="f30eb-214">(Nebo odebrat) hello udev pravidla tooavoid generování statická pravidla pro rozhraní sítě Ethernet hello.</span><span class="sxs-lookup"><span data-stu-id="f30eb-214">Move (or remove) hello udev rules tooavoid generating static rules for hello Ethernet interface.</span></span> <span data-ttu-id="f30eb-215">Tato pravidla způsobit problémy při klonování virtuálního počítače v Azure nebo Hyper-V:</span><span class="sxs-lookup"><span data-stu-id="f30eb-215">These rules cause problems when you clone a virtual machine in Azure or Hyper-V:</span></span>

        # sudo ln -s /dev/null /etc/udev/rules.d/75-persistent-net-generator.rules

        # sudo rm -f /etc/udev/rules.d/70-persistent-net.rules

7. <span data-ttu-id="f30eb-216">Ujistěte se, že hello síťová služba se spustí při spuštění spuštěním hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="f30eb-216">Ensure that hello network service will start at boot time by running hello following command:</span></span>

        # chkconfig network on

8. <span data-ttu-id="f30eb-217">Zaregistrujte předplatné Red Hat tooenable hello instalace balíčků z úložiště RHEL hello spuštěním hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="f30eb-217">Register your Red Hat subscription tooenable hello installation of packages from hello RHEL repository by running hello following command:</span></span>

        # subscription-manager register --auto-attach --username=XXX --password=XXX

9. <span data-ttu-id="f30eb-218">Upravte hello jádra spouštěcí řádku grub konfigurace tooinclude další jádra parametry pro Azure.</span><span class="sxs-lookup"><span data-stu-id="f30eb-218">Modify hello kernel boot line in your grub configuration tooinclude additional kernel parameters for Azure.</span></span> <span data-ttu-id="f30eb-219">toodo tuto konfiguraci, otevřete `/boot/grub/menu.lst` v textovém editoru a ujistěte se, že jádra výchozí hello zahrnuje hello následující parametry:</span><span class="sxs-lookup"><span data-stu-id="f30eb-219">toodo this configuration, open `/boot/grub/menu.lst` in a text editor, and ensure that hello default kernel includes hello following parameters:</span></span>
    
        console=ttyS0 earlyprintk=ttyS0 rootdelay=300
    
    <span data-ttu-id="f30eb-220">To také zajistí, že všechny zprávy konzoly jsou odesílány toohello první sériového portu, který může být užitečné Azure podporu pro ladění problémů.</span><span class="sxs-lookup"><span data-stu-id="f30eb-220">This will also ensure that all console messages are sent toohello first serial port, which can assist Azure support with debugging issues.</span></span>
    
    <span data-ttu-id="f30eb-221">Kromě toho doporučujeme odebrat hello následující parametry:</span><span class="sxs-lookup"><span data-stu-id="f30eb-221">In addition, we recommend that you remove hello following parameters:</span></span>
    
        rhgb quiet crashkernel=auto
    
    <span data-ttu-id="f30eb-222">Grafické a quiet spouštěcí nejsou užitečné při cloudovém prostředí, kde chceme, aby všechny protokoly toobe hello odeslané toohello sériového portu.</span><span class="sxs-lookup"><span data-stu-id="f30eb-222">Graphical and quiet boot are not useful in a cloud environment where we want all hello logs toobe sent toohello serial port.</span></span> <span data-ttu-id="f30eb-223">Můžete ponechat hello `crashkernel` možnost nakonfigurované v případě potřeby.</span><span class="sxs-lookup"><span data-stu-id="f30eb-223">You can leave hello `crashkernel` option configured if desired.</span></span> <span data-ttu-id="f30eb-224">Všimněte si, že tento parametr snižuje hello množství dostupné paměti v hello virtuálního počítače pomocí 128 MB nebo víc, což může způsobovat problémy menší velikostí virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="f30eb-224">Note that this parameter reduces hello amount of available memory in hello virtual machine by 128 MB or more, which might be problematic on smaller virtual machine sizes.</span></span>

    >[!Important]
    <span data-ttu-id="f30eb-225">RHEL verze 6.5 a starší, musíte taky nastavit hello `numa=off` jádra parametr.</span><span class="sxs-lookup"><span data-stu-id="f30eb-225">RHEL 6.5 and earlier must also set hello `numa=off` kernel parameter.</span></span> <span data-ttu-id="f30eb-226">V tématu Red Hat [KB 436883](https://access.redhat.com/solutions/436883).</span><span class="sxs-lookup"><span data-stu-id="f30eb-226">See Red Hat [KB 436883](https://access.redhat.com/solutions/436883).</span></span>

10. <span data-ttu-id="f30eb-227">Přidejte tooinitramfs modulů technologie Hyper-V:</span><span class="sxs-lookup"><span data-stu-id="f30eb-227">Add Hyper-V modules tooinitramfs:</span></span>  

    <span data-ttu-id="f30eb-228">Upravit `/etc/dracut.conf`a přidejte hello následující obsah:</span><span class="sxs-lookup"><span data-stu-id="f30eb-228">Edit `/etc/dracut.conf`, and add hello following content:</span></span>

        add_drivers+="hv_vmbus hv_netvsc hv_storvsc"

    <span data-ttu-id="f30eb-229">Znovu sestavte initramfs:</span><span class="sxs-lookup"><span data-stu-id="f30eb-229">Rebuild initramfs:</span></span>

        # dracut -f -v

11. <span data-ttu-id="f30eb-230">Odinstalace init cloudu:</span><span class="sxs-lookup"><span data-stu-id="f30eb-230">Uninstall cloud-init:</span></span>

        # yum remove cloud-init

12. <span data-ttu-id="f30eb-231">Ujistěte se, že server SSH hello je nainstalován a nakonfigurován toostart při spuštění:</span><span class="sxs-lookup"><span data-stu-id="f30eb-231">Ensure that hello SSH server is installed and configured toostart at boot time:</span></span>

        # chkconfig sshd on

    <span data-ttu-id="f30eb-232">Upravte hello tooinclude /etc/ssh/sshd_config následující řádky:</span><span class="sxs-lookup"><span data-stu-id="f30eb-232">Modify /etc/ssh/sshd_config tooinclude hello following lines:</span></span>

        PasswordAuthentication yes
        ClientAliveInterval 180

13. <span data-ttu-id="f30eb-233">balíček WALinuxAgent Hello `WALinuxAgent-<version>`, bylo posunuto toohello Red Hat funkce úložiště.</span><span class="sxs-lookup"><span data-stu-id="f30eb-233">hello WALinuxAgent package, `WALinuxAgent-<version>`, has been pushed toohello Red Hat extras repository.</span></span> <span data-ttu-id="f30eb-234">Povolení funkce úložiště hello spuštěním hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="f30eb-234">Enable hello extras repository by running hello following command:</span></span>

        # subscription-manager repos --enable=rhel-6-server-extras-rpms

14. <span data-ttu-id="f30eb-235">Nainstalujte hello Azure Linux Agent spuštěním hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="f30eb-235">Install hello Azure Linux Agent by running hello following command:</span></span>

        # yum install WALinuxAgent

        # chkconfig waagent on

15. <span data-ttu-id="f30eb-236">Hello Azure Linux Agent mohou automaticky konfigurovat odkládacího souboru pomocí disku hello místní prostředek, který je připojený toohello virtuální počítač po zřízení hello virtuálního počítače na platformě Azure.</span><span class="sxs-lookup"><span data-stu-id="f30eb-236">hello Azure Linux Agent can automatically configure swap space by using hello local resource disk that is attached toohello virtual machine after hello virtual machine is provisioned on Azure.</span></span> <span data-ttu-id="f30eb-237">Upozorňujeme, že hello místní prostředek disk je dočasný a může být vyprázdnit po deprovisioned hello virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="f30eb-237">Note that hello local resource disk is a temporary disk, and it might be emptied when hello virtual machine is deprovisioned.</span></span> <span data-ttu-id="f30eb-238">Po instalaci hello Azure Linux Agent v předchozím kroku hello se upravit hello následující parametry v **/etc/waagent.conf** odpovídajícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="f30eb-238">After you install hello Azure Linux Agent in hello previous step, modify hello following parameters in **/etc/waagent.conf** appropriately:</span></span>

        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this toowhatever you need it toobe.

16. <span data-ttu-id="f30eb-239">Zrušení registrace předplatného hello (v případě potřeby) tak, že spustíte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="f30eb-239">Unregister hello subscription (if necessary) by running hello following command:</span></span>

        # subscription-manager unregister

17. <span data-ttu-id="f30eb-240">Spusťte hello následující příkazy toodeprovision hello virtuálního počítače a jeho přípravu pro zřizování na Azure:</span><span class="sxs-lookup"><span data-stu-id="f30eb-240">Run hello following commands toodeprovision hello virtual machine and prepare it for provisioning on Azure:</span></span>

        # waagent -force -deprovision

        # export HISTSIZE=0

        # logout

18. <span data-ttu-id="f30eb-241">Vypnete virtuální počítač hello v KVM.</span><span class="sxs-lookup"><span data-stu-id="f30eb-241">Shut down hello virtual machine in KVM.</span></span>

19. <span data-ttu-id="f30eb-242">Převod formátu virtuálního pevného disku toohello image qcow2 hello.</span><span class="sxs-lookup"><span data-stu-id="f30eb-242">Convert hello qcow2 image toohello VHD format.</span></span>

    <span data-ttu-id="f30eb-243">Nejprve převeďte formát tooraw hello obrázku:</span><span class="sxs-lookup"><span data-stu-id="f30eb-243">First convert hello image tooraw format:</span></span>

        # qemu-img convert -f qcow2 -O raw rhel-6.8.qcow2 rhel-6.8.raw

    <span data-ttu-id="f30eb-244">Ujistěte se, že velikost hello hello nezpracovaná bitové kopie je v souladu s 1 MB.</span><span class="sxs-lookup"><span data-stu-id="f30eb-244">Make sure that hello size of hello raw image is aligned with 1 MB.</span></span> <span data-ttu-id="f30eb-245">Jinak zaokrouhlí nahoru hello velikost tooalign s 1 MB:</span><span class="sxs-lookup"><span data-stu-id="f30eb-245">Otherwise, round up hello size tooalign with 1 MB:</span></span>

        # MB=$((1024*1024))
        # size=$(qemu-img info -f raw --output json "rhel-6.8.raw" | \
          gawk 'match($0, /"virtual-size": ([0-9]+),/, val) {print val[1]}')

        # rounded_size=$((($size/$MB + 1)*$MB))
        # qemu-img resize rhel-6.8.raw $rounded_size

    <span data-ttu-id="f30eb-246">Převést tooa hello nezpracovaná disku pevné velikosti virtuálního pevného disku:</span><span class="sxs-lookup"><span data-stu-id="f30eb-246">Convert hello raw disk tooa fixed-sized VHD:</span></span>

        # qemu-img convert -f raw -o subformat=fixed -O vpc rhel-6.8.raw rhel-6.8.vhd


### <a name="prepare-a-rhel-7-virtual-machine-from-kvm"></a><span data-ttu-id="f30eb-247">Příprava virtuálního počítače, RHEL 7 z KVM</span><span class="sxs-lookup"><span data-stu-id="f30eb-247">Prepare a RHEL 7 virtual machine from KVM</span></span>

1. <span data-ttu-id="f30eb-248">Obrázek KVM hello RHEL 7 stáhněte z webu Red Hat hello.</span><span class="sxs-lookup"><span data-stu-id="f30eb-248">Download hello KVM image of RHEL 7 from hello Red Hat website.</span></span> <span data-ttu-id="f30eb-249">Tento postup používá jako příklad hello RHEL 7.</span><span class="sxs-lookup"><span data-stu-id="f30eb-249">This procedure uses RHEL 7 as hello example.</span></span>

2. <span data-ttu-id="f30eb-250">Nastavení kořenové heslo.</span><span class="sxs-lookup"><span data-stu-id="f30eb-250">Set a root password.</span></span>

    <span data-ttu-id="f30eb-251">Generovat zašifrované heslo a zkopírujte hello výstup hello příkazu:</span><span class="sxs-lookup"><span data-stu-id="f30eb-251">Generate an encrypted password, and copy hello output of hello command:</span></span>

        # openssl passwd -1 changeme

    <span data-ttu-id="f30eb-252">Nastavení kořenové heslo s guestfish:</span><span class="sxs-lookup"><span data-stu-id="f30eb-252">Set a root password with guestfish:</span></span>

        # guestfish --rw -a <image-name>
        > <fs> run
        > <fs> list-filesystems
        > <fs> mount /dev/sda1 /
        > <fs> vi /etc/shadow
        > <fs> exit

   <span data-ttu-id="f30eb-253">Změna hello druhé pole kořenové uživatele z "!"</span><span class="sxs-lookup"><span data-stu-id="f30eb-253">Change hello second field of root user from "!!"</span></span> <span data-ttu-id="f30eb-254">toohello zašifrované heslo.</span><span class="sxs-lookup"><span data-stu-id="f30eb-254">toohello encrypted password.</span></span>

3. <span data-ttu-id="f30eb-255">Vytvoření virtuálního počítače v KVM z hello qcow2 image.</span><span class="sxs-lookup"><span data-stu-id="f30eb-255">Create a virtual machine in KVM from hello qcow2 image.</span></span> <span data-ttu-id="f30eb-256">Nastavte typ disku hello příliš**qcow2**a nastavte model zařízení rozhraní virtuální sítě hello příliš**virtio**.</span><span class="sxs-lookup"><span data-stu-id="f30eb-256">Set hello disk type too**qcow2**, and set hello virtual network interface device model too**virtio**.</span></span> <span data-ttu-id="f30eb-257">Potom spusťte hello virtuálního počítače a přihlaste se jako kořenový.</span><span class="sxs-lookup"><span data-stu-id="f30eb-257">Then, start hello virtual machine, and sign in as root.</span></span>

4. <span data-ttu-id="f30eb-258">Vytvořte nebo upravte hello `/etc/sysconfig/network` souboru a přidejte hello následující text:</span><span class="sxs-lookup"><span data-stu-id="f30eb-258">Create or edit hello `/etc/sysconfig/network` file, and add hello following text:</span></span>
   
        NETWORKING=yes
        HOSTNAME=localhost.localdomain

5. <span data-ttu-id="f30eb-259">Vytvořte nebo upravte hello `/etc/sysconfig/network-scripts/ifcfg-eth0` souboru a přidejte hello následující text:</span><span class="sxs-lookup"><span data-stu-id="f30eb-259">Create or edit hello `/etc/sysconfig/network-scripts/ifcfg-eth0` file, and add hello following text:</span></span>
   
        DEVICE=eth0
        ONBOOT=yes
        BOOTPROTO=dhcp
        TYPE=Ethernet
        USERCTL=no
        PEERDNS=yes
        IPV6INIT=no
        NM_CONTROLLED=no

6. <span data-ttu-id="f30eb-260">Ujistěte se, že hello síťová služba se spustí při spuštění spuštěním hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="f30eb-260">Ensure that hello network service will start at boot time by running hello following command:</span></span>

        # chkconfig network on

7. <span data-ttu-id="f30eb-261">Zaregistrujte předplatné Red Hat tooenable instalace balíčků z úložiště RHEL hello spuštěním hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="f30eb-261">Register your Red Hat subscription tooenable installation of packages from hello RHEL repository by running hello following command:</span></span>

        # subscription-manager register --auto-attach --username=XXX --password=XXX

8. <span data-ttu-id="f30eb-262">Upravte hello jádra spouštěcí řádku grub konfigurace tooinclude další jádra parametry pro Azure.</span><span class="sxs-lookup"><span data-stu-id="f30eb-262">Modify hello kernel boot line in your grub configuration tooinclude additional kernel parameters for Azure.</span></span> <span data-ttu-id="f30eb-263">toodo tuto konfiguraci, otevřete `/etc/default/grub` v textovém editoru a upravit hello `GRUB_CMDLINE_LINUX` parametr.</span><span class="sxs-lookup"><span data-stu-id="f30eb-263">toodo this configuration, open `/etc/default/grub` in a text editor, and edit hello `GRUB_CMDLINE_LINUX` parameter.</span></span> <span data-ttu-id="f30eb-264">Například:</span><span class="sxs-lookup"><span data-stu-id="f30eb-264">For example:</span></span>
   
        GRUB_CMDLINE_LINUX="rootdelay=300 console=ttyS0 earlyprintk=ttyS0 net.ifnames=0"
   
   <span data-ttu-id="f30eb-265">Tento příkaz také zajistí, že všechny zprávy konzoly jsou odesílány toohello první sériového portu, který může být užitečné Azure podporu pro ladění problémů.</span><span class="sxs-lookup"><span data-stu-id="f30eb-265">This command also ensures that all console messages are sent toohello first serial port, which can assist Azure support with debugging issues.</span></span> <span data-ttu-id="f30eb-266">příkaz Hello vypne také hello nové RHEL 7 zásady vytváření názvů pro síťové adaptéry.</span><span class="sxs-lookup"><span data-stu-id="f30eb-266">hello command also turns off hello new RHEL 7 naming conventions for NICs.</span></span> <span data-ttu-id="f30eb-267">Kromě toho doporučujeme odebrat hello následující parametry:</span><span class="sxs-lookup"><span data-stu-id="f30eb-267">In addition, we recommend that you remove hello following parameters:</span></span>
   
        rhgb quiet crashkernel=auto
   
    <span data-ttu-id="f30eb-268">Grafické a quiet spouštěcí nejsou užitečné při cloudovém prostředí, kde chceme, aby všechny protokoly toobe hello odeslané toohello sériového portu.</span><span class="sxs-lookup"><span data-stu-id="f30eb-268">Graphical and quiet boot are not useful in a cloud environment where we want all hello logs toobe sent toohello serial port.</span></span> <span data-ttu-id="f30eb-269">Můžete ponechat hello `crashkernel` možnost nakonfigurované v případě potřeby.</span><span class="sxs-lookup"><span data-stu-id="f30eb-269">You can leave hello `crashkernel` option configured if desired.</span></span> <span data-ttu-id="f30eb-270">Všimněte si, že tento parametr snižuje hello množství dostupné paměti v hello virtuálního počítače pomocí 128 MB nebo víc, což může způsobovat problémy menší velikostí virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="f30eb-270">Note that this parameter reduces hello amount of available memory in hello virtual machine by 128 MB or more, which might be problematic on smaller virtual machine sizes.</span></span>

9. <span data-ttu-id="f30eb-271">Po dokončení úprav `/etc/default/grub`spusťte hello následující příkaz toorebuild hello grub konfigurace:</span><span class="sxs-lookup"><span data-stu-id="f30eb-271">After you are done editing `/etc/default/grub`, run hello following command toorebuild hello grub configuration:</span></span>

        # grub2-mkconfig -o /boot/grub2/grub.cfg

10. <span data-ttu-id="f30eb-272">Přidejte do initramfs moduly Hyper-V.</span><span class="sxs-lookup"><span data-stu-id="f30eb-272">Add Hyper-V modules into initramfs.</span></span>

    <span data-ttu-id="f30eb-273">Upravit `/etc/dracut.conf` a přidejte obsah:</span><span class="sxs-lookup"><span data-stu-id="f30eb-273">Edit `/etc/dracut.conf` and add content:</span></span>

        add_drivers+="hv_vmbus hv_netvsc hv_storvsc"

    <span data-ttu-id="f30eb-274">Znovu sestavte initramfs:</span><span class="sxs-lookup"><span data-stu-id="f30eb-274">Rebuild initramfs:</span></span>

        # dracut -f -v

11. <span data-ttu-id="f30eb-275">Odinstalace init cloudu:</span><span class="sxs-lookup"><span data-stu-id="f30eb-275">Uninstall cloud-init:</span></span>

        # yum remove cloud-init

12. <span data-ttu-id="f30eb-276">Ujistěte se, že server SSH hello je nainstalován a nakonfigurován toostart při spuštění:</span><span class="sxs-lookup"><span data-stu-id="f30eb-276">Ensure that hello SSH server is installed and configured toostart at boot time:</span></span>

        # systemctl enable sshd

    <span data-ttu-id="f30eb-277">Upravte hello tooinclude /etc/ssh/sshd_config následující řádky:</span><span class="sxs-lookup"><span data-stu-id="f30eb-277">Modify /etc/ssh/sshd_config tooinclude hello following lines:</span></span>

        PasswordAuthentication yes
        ClientAliveInterval 180

13. <span data-ttu-id="f30eb-278">balíček WALinuxAgent Hello `WALinuxAgent-<version>`, bylo posunuto toohello Red Hat funkce úložiště.</span><span class="sxs-lookup"><span data-stu-id="f30eb-278">hello WALinuxAgent package, `WALinuxAgent-<version>`, has been pushed toohello Red Hat extras repository.</span></span> <span data-ttu-id="f30eb-279">Povolení funkce úložiště hello spuštěním hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="f30eb-279">Enable hello extras repository by running hello following command:</span></span>

        # subscription-manager repos --enable=rhel-7-server-extras-rpms

14. <span data-ttu-id="f30eb-280">Nainstalujte hello Azure Linux Agent spuštěním hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="f30eb-280">Install hello Azure Linux Agent by running hello following command:</span></span>

        # yum install WALinuxAgent

    <span data-ttu-id="f30eb-281">Povolení služby příkaz waagent hello:</span><span class="sxs-lookup"><span data-stu-id="f30eb-281">Enable hello waagent service:</span></span>

        # systemctl enable waagent.service

15. <span data-ttu-id="f30eb-282">Nevytvářejte odkládacího prostoru na disku operačního systému hello.</span><span class="sxs-lookup"><span data-stu-id="f30eb-282">Do not create swap space on hello operating system disk.</span></span>

    <span data-ttu-id="f30eb-283">Hello Azure Linux Agent mohou automaticky konfigurovat odkládacího souboru pomocí disku hello místní prostředek, který je připojený toohello virtuální počítač po zřízení hello virtuálního počítače na platformě Azure.</span><span class="sxs-lookup"><span data-stu-id="f30eb-283">hello Azure Linux Agent can automatically configure swap space by using hello local resource disk that is attached toohello virtual machine after hello virtual machine is provisioned on Azure.</span></span> <span data-ttu-id="f30eb-284">Upozorňujeme, že hello místní prostředek disk je dočasný a může být vyprázdnit po deprovisioned hello virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="f30eb-284">Note that hello local resource disk is a temporary disk, and it might be emptied when hello virtual machine is deprovisioned.</span></span> <span data-ttu-id="f30eb-285">Po instalaci hello Azure Linux Agent v předchozím kroku hello se upravit hello následující parametry v `/etc/waagent.conf` odpovídajícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="f30eb-285">After you install hello Azure Linux Agent in hello previous step, modify hello following parameters in `/etc/waagent.conf` appropriately:</span></span>

        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this toowhatever you need it toobe.

16. <span data-ttu-id="f30eb-286">Zrušení registrace předplatného hello (v případě potřeby) tak, že spustíte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="f30eb-286">Unregister hello subscription (if necessary) by running hello following command:</span></span>

        # subscription-manager unregister

17. <span data-ttu-id="f30eb-287">Spusťte hello následující příkazy toodeprovision hello virtuálního počítače a jeho přípravu pro zřizování na Azure:</span><span class="sxs-lookup"><span data-stu-id="f30eb-287">Run hello following commands toodeprovision hello virtual machine and prepare it for provisioning on Azure:</span></span>

        # sudo waagent -force -deprovision

        # export HISTSIZE=0

        # logout

18. <span data-ttu-id="f30eb-288">Vypnete virtuální počítač hello v KVM.</span><span class="sxs-lookup"><span data-stu-id="f30eb-288">Shut down hello virtual machine in KVM.</span></span>

19. <span data-ttu-id="f30eb-289">Převod formátu virtuálního pevného disku toohello image qcow2 hello.</span><span class="sxs-lookup"><span data-stu-id="f30eb-289">Convert hello qcow2 image toohello VHD format.</span></span>

    <span data-ttu-id="f30eb-290">Nejprve převeďte formát tooraw hello obrázku:</span><span class="sxs-lookup"><span data-stu-id="f30eb-290">First convert hello image tooraw format:</span></span>

        # qemu-img convert -f qcow2 -O raw rhel-7.3.qcow2 rhel-7.3.raw

    <span data-ttu-id="f30eb-291">Ujistěte se, že velikost hello hello nezpracovaná bitové kopie je v souladu s 1 MB.</span><span class="sxs-lookup"><span data-stu-id="f30eb-291">Make sure that hello size of hello raw image is aligned with 1 MB.</span></span> <span data-ttu-id="f30eb-292">Jinak zaokrouhlí nahoru hello velikost tooalign s 1 MB:</span><span class="sxs-lookup"><span data-stu-id="f30eb-292">Otherwise, round up hello size tooalign with 1 MB:</span></span>

        # MB=$((1024*1024))
        # size=$(qemu-img info -f raw --output json "rhel-6.8.raw" | \
          gawk 'match($0, /"virtual-size": ([0-9]+),/, val) {print val[1]}')

        # rounded_size=$((($size/$MB + 1)*$MB))
        # qemu-img resize rhel-6.8.raw $rounded_size

    <span data-ttu-id="f30eb-293">Převést tooa hello nezpracovaná disku pevné velikosti virtuálního pevného disku:</span><span class="sxs-lookup"><span data-stu-id="f30eb-293">Convert hello raw disk tooa fixed-sized VHD:</span></span>

        # qemu-img convert -f raw -o subformat=fixed -O vpc rhel-7.3.raw rhel-7.3.vhd

## <a name="prepare-a-red-hat-based-virtual-machine-from-vmware"></a><span data-ttu-id="f30eb-294">Příprava virtuálního počítače, na základě Red Hat z VMware</span><span class="sxs-lookup"><span data-stu-id="f30eb-294">Prepare a Red Hat-based virtual machine from VMware</span></span>
### <a name="prerequisites"></a><span data-ttu-id="f30eb-295">Požadavky</span><span class="sxs-lookup"><span data-stu-id="f30eb-295">Prerequisites</span></span>
<span data-ttu-id="f30eb-296">V této části se předpokládá, že jste již nainstalovali RHEL virtuálního počítače v prostředí VMware.</span><span class="sxs-lookup"><span data-stu-id="f30eb-296">This section assumes that you have already installed a RHEL virtual machine in VMware.</span></span> <span data-ttu-id="f30eb-297">Podrobnosti o tom, jak tooinstall operačního systému v prostředí VMware, najdete v části [Průvodce instalací operačního systému hosta VMware](http://partnerweb.vmware.com/GOSIG/home.html).</span><span class="sxs-lookup"><span data-stu-id="f30eb-297">For details about how tooinstall an operating system in VMware, see [VMware Guest Operating System Installation Guide](http://partnerweb.vmware.com/GOSIG/home.html).</span></span>

* <span data-ttu-id="f30eb-298">Při instalaci operačního systému Linux hello, doporučujeme použít standardní oddíly spíše než LVM, což je často hello výchozí pro mnoho instalace.</span><span class="sxs-lookup"><span data-stu-id="f30eb-298">When you install hello Linux operating system, we recommend that you use standard partitions rather than LVM, which is often hello default for many installations.</span></span> <span data-ttu-id="f30eb-299">Tím se vyhnete LVM název je v konfliktu s naklonovaný virtuální počítač, zvlášť pokud disk operačního systému pro řešení potíží s někdy potřebuje toobe připojené tooanother virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="f30eb-299">This will avoid LVM name conflicts with cloned virtual machine, particularly if an operating system disk ever needs toobe attached tooanother virtual machine for troubleshooting.</span></span> <span data-ttu-id="f30eb-300">LVM nebo RAID lze použít v datových disků Pokud dáváte přednost.</span><span class="sxs-lookup"><span data-stu-id="f30eb-300">LVM or RAID can be used on data disks if preferred.</span></span>
* <span data-ttu-id="f30eb-301">Nekonfigurujte přepnutí oddílu na disku operačního systému hello.</span><span class="sxs-lookup"><span data-stu-id="f30eb-301">Do not configure a swap partition on hello operating system disk.</span></span> <span data-ttu-id="f30eb-302">Můžete nakonfigurovat hello Linux agent toocreate odkládací soubor na disku hello dočasných prostředků.</span><span class="sxs-lookup"><span data-stu-id="f30eb-302">You can configure hello Linux agent toocreate a swap file on hello temporary resource disk.</span></span> <span data-ttu-id="f30eb-303">Další informace o tom najdete v hello kroky, které následují.</span><span class="sxs-lookup"><span data-stu-id="f30eb-303">You can find more information about this in hello steps that follow.</span></span>
* <span data-ttu-id="f30eb-304">Když vytvoříte hello virtuální pevný disk, vyberte **virtuálního disku úložiště jako samostatný soubor**.</span><span class="sxs-lookup"><span data-stu-id="f30eb-304">When you create hello virtual hard disk, select **Store virtual disk as a single file**.</span></span>

### <a name="prepare-a-rhel-6-virtual-machine-from-vmware"></a><span data-ttu-id="f30eb-305">Příprava systému RHEL 6 virtuálního počítače z VMware</span><span class="sxs-lookup"><span data-stu-id="f30eb-305">Prepare a RHEL 6 virtual machine from VMware</span></span>
1. <span data-ttu-id="f30eb-306">V systému RHEL 6 NetworkManager může narušovat hello Azure Linux agent.</span><span class="sxs-lookup"><span data-stu-id="f30eb-306">In RHEL 6, NetworkManager can interfere with hello Azure Linux agent.</span></span> <span data-ttu-id="f30eb-307">Odinstalaci tohoto balíčku spuštěním hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="f30eb-307">Uninstall this package by running hello following command:</span></span>
   
        # sudo rpm -e --nodeps NetworkManager

2. <span data-ttu-id="f30eb-308">Vytvořte soubor s názvem **sítě** v hello etc/sysconfig/adresář, který obsahuje hello následující text:</span><span class="sxs-lookup"><span data-stu-id="f30eb-308">Create a file named **network** in hello /etc/sysconfig/ directory that contains hello following text:</span></span>

        NETWORKING=yes
        HOSTNAME=localhost.localdomain

3. <span data-ttu-id="f30eb-309">Vytvořte nebo upravte hello `/etc/sysconfig/network-scripts/ifcfg-eth0` souboru a přidejte hello následující text:</span><span class="sxs-lookup"><span data-stu-id="f30eb-309">Create or edit hello `/etc/sysconfig/network-scripts/ifcfg-eth0` file, and add hello following text:</span></span>
   
        DEVICE=eth0
        ONBOOT=yes
        BOOTPROTO=dhcp
        TYPE=Ethernet
        USERCTL=no
        PEERDNS=yes
        IPV6INIT=no

4. <span data-ttu-id="f30eb-310">(Nebo odebrat) hello udev pravidla tooavoid generování statická pravidla pro rozhraní sítě Ethernet hello.</span><span class="sxs-lookup"><span data-stu-id="f30eb-310">Move (or remove) hello udev rules tooavoid generating static rules for hello Ethernet interface.</span></span> <span data-ttu-id="f30eb-311">Tato pravidla způsobit problémy při klonování virtuálního počítače v Azure nebo Hyper-V:</span><span class="sxs-lookup"><span data-stu-id="f30eb-311">These rules cause problems when you clone a virtual machine in Azure or Hyper-V:</span></span>

        # sudo ln -s /dev/null /etc/udev/rules.d/75-persistent-net-generator.rules

        # sudo rm -f /etc/udev/rules.d/70-persistent-net.rules

5. <span data-ttu-id="f30eb-312">Ujistěte se, že hello síťová služba se spustí při spuštění spuštěním hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="f30eb-312">Ensure that hello network service will start at boot time by running hello following command:</span></span>

        # sudo chkconfig network on

6. <span data-ttu-id="f30eb-313">Zaregistrujte předplatné Red Hat tooenable hello instalace balíčků z úložiště RHEL hello spuštěním hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="f30eb-313">Register your Red Hat subscription tooenable hello installation of packages from hello RHEL repository by running hello following command:</span></span>

        # sudo subscription-manager register --auto-attach --username=XXX --password=XXX

7. <span data-ttu-id="f30eb-314">balíček WALinuxAgent Hello `WALinuxAgent-<version>`, bylo posunuto toohello Red Hat funkce úložiště.</span><span class="sxs-lookup"><span data-stu-id="f30eb-314">hello WALinuxAgent package, `WALinuxAgent-<version>`, has been pushed toohello Red Hat extras repository.</span></span> <span data-ttu-id="f30eb-315">Povolení funkce úložiště hello spuštěním hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="f30eb-315">Enable hello extras repository by running hello following command:</span></span>

        # subscription-manager repos --enable=rhel-6-server-extras-rpms

8. <span data-ttu-id="f30eb-316">Upravte hello jádra spouštěcí řádku grub konfigurace tooinclude další jádra parametry pro Azure.</span><span class="sxs-lookup"><span data-stu-id="f30eb-316">Modify hello kernel boot line in your grub configuration tooinclude additional kernel parameters for Azure.</span></span> <span data-ttu-id="f30eb-317">toodo tuto, otevřete `/etc/default/grub` v textovém editoru a upravit hello `GRUB_CMDLINE_LINUX` parametr.</span><span class="sxs-lookup"><span data-stu-id="f30eb-317">toodo this, open `/etc/default/grub` in a text editor, and edit hello `GRUB_CMDLINE_LINUX` parameter.</span></span> <span data-ttu-id="f30eb-318">Například:</span><span class="sxs-lookup"><span data-stu-id="f30eb-318">For example:</span></span>
   
        GRUB_CMDLINE_LINUX="rootdelay=300 console=ttyS0 earlyprintk=ttyS0"
   
   <span data-ttu-id="f30eb-319">To také zajistí, že všechny zprávy konzoly jsou odesílány toohello první sériového portu, který může být užitečné Azure podporu pro ladění problémů.</span><span class="sxs-lookup"><span data-stu-id="f30eb-319">This will also ensure that all console messages are sent toohello first serial port, which can assist Azure support with debugging issues.</span></span> <span data-ttu-id="f30eb-320">Kromě toho doporučujeme odebrat hello následující parametry:</span><span class="sxs-lookup"><span data-stu-id="f30eb-320">In addition, we recommend that you remove hello following parameters:</span></span>
   
        rhgb quiet crashkernel=auto
   
    <span data-ttu-id="f30eb-321">Grafické a quiet spouštěcí nejsou užitečné při cloudovém prostředí, kde chceme, aby všechny protokoly toobe hello odeslané toohello sériového portu.</span><span class="sxs-lookup"><span data-stu-id="f30eb-321">Graphical and quiet boot are not useful in a cloud environment where we want all hello logs toobe sent toohello serial port.</span></span> <span data-ttu-id="f30eb-322">Můžete ponechat hello `crashkernel` možnost nakonfigurované v případě potřeby.</span><span class="sxs-lookup"><span data-stu-id="f30eb-322">You can leave hello `crashkernel` option configured if desired.</span></span> <span data-ttu-id="f30eb-323">Všimněte si, že tento parametr snižuje hello množství dostupné paměti v hello virtuálního počítače pomocí 128 MB nebo víc, což může způsobovat problémy menší velikostí virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="f30eb-323">Note that this parameter reduces hello amount of available memory in hello virtual machine by 128 MB or more, which might be problematic on smaller virtual machine sizes.</span></span>

9. <span data-ttu-id="f30eb-324">Přidejte tooinitramfs modulů technologie Hyper-V:</span><span class="sxs-lookup"><span data-stu-id="f30eb-324">Add Hyper-V modules tooinitramfs:</span></span>

    <span data-ttu-id="f30eb-325">Upravit `/etc/dracut.conf`a přidejte hello následující obsah:</span><span class="sxs-lookup"><span data-stu-id="f30eb-325">Edit `/etc/dracut.conf`, and add hello following content:</span></span>

        add_drivers+="hv_vmbus hv_netvsc hv_storvsc"

    <span data-ttu-id="f30eb-326">Znovu sestavte initramfs:</span><span class="sxs-lookup"><span data-stu-id="f30eb-326">Rebuild initramfs:</span></span>

        # dracut -f -v

10. <span data-ttu-id="f30eb-327">Ujistěte se, že hello SSH server je nainstalován a nakonfigurován toostart při spuštění, který je obvykle výchozí hello.</span><span class="sxs-lookup"><span data-stu-id="f30eb-327">Ensure that hello SSH server is installed and configured toostart at boot time, which is usually hello default.</span></span> <span data-ttu-id="f30eb-328">Upravit `/etc/ssh/sshd_config` tooinclude hello následující řádek:</span><span class="sxs-lookup"><span data-stu-id="f30eb-328">Modify `/etc/ssh/sshd_config` tooinclude hello following line:</span></span>

    <span data-ttu-id="f30eb-329">ClientAliveInterval 180</span><span class="sxs-lookup"><span data-stu-id="f30eb-329">ClientAliveInterval 180</span></span>

11. <span data-ttu-id="f30eb-330">Nainstalujte hello Azure Linux Agent spuštěním hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="f30eb-330">Install hello Azure Linux Agent by running hello following command:</span></span>

        # sudo yum install WALinuxAgent

        # sudo chkconfig waagent on

12. <span data-ttu-id="f30eb-331">Nevytvářejte odkládacího prostoru na disku operačního systému hello.</span><span class="sxs-lookup"><span data-stu-id="f30eb-331">Do not create swap space on hello operating system disk.</span></span>

    <span data-ttu-id="f30eb-332">Hello Azure Linux Agent mohou automaticky konfigurovat odkládacího souboru pomocí disku hello místní prostředek, který je připojený toohello virtuální počítač po zřízení hello virtuálního počítače na platformě Azure.</span><span class="sxs-lookup"><span data-stu-id="f30eb-332">hello Azure Linux Agent can automatically configure swap space by using hello local resource disk that is attached toohello virtual machine after hello virtual machine is provisioned on Azure.</span></span> <span data-ttu-id="f30eb-333">Upozorňujeme, že hello místní prostředek disk je dočasný a může být vyprázdnit po deprovisioned hello virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="f30eb-333">Note that hello local resource disk is a temporary disk, and it might be emptied when hello virtual machine is deprovisioned.</span></span> <span data-ttu-id="f30eb-334">Po instalaci hello Azure Linux Agent v předchozím kroku hello se upravit hello následující parametry v `/etc/waagent.conf` odpovídajícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="f30eb-334">After you install hello Azure Linux Agent in hello previous step, modify hello following parameters in `/etc/waagent.conf` appropriately:</span></span>

        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this toowhatever you need it toobe.

13. <span data-ttu-id="f30eb-335">Zrušení registrace předplatného hello (v případě potřeby) tak, že spustíte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="f30eb-335">Unregister hello subscription (if necessary) by running hello following command:</span></span>

        # sudo subscription-manager unregister

14. <span data-ttu-id="f30eb-336">Spusťte hello následující příkazy toodeprovision hello virtuálního počítače a jeho přípravu pro zřizování na Azure:</span><span class="sxs-lookup"><span data-stu-id="f30eb-336">Run hello following commands toodeprovision hello virtual machine and prepare it for provisioning on Azure:</span></span>

        # sudo waagent -force -deprovision

        # export HISTSIZE=0

        # logout

15. <span data-ttu-id="f30eb-337">Vypnout hello virtuální počítač a proveďte převod souboru VHD tooa soubor VMDK hello.</span><span class="sxs-lookup"><span data-stu-id="f30eb-337">Shut down hello virtual machine, and convert hello VMDK file tooa .vhd file.</span></span>

    <span data-ttu-id="f30eb-338">Nejprve převeďte formát tooraw hello obrázku:</span><span class="sxs-lookup"><span data-stu-id="f30eb-338">First convert hello image tooraw format:</span></span>

        # qemu-img convert -f vmdk -O raw rhel-6.8.vmdk rhel-6.8.raw

    <span data-ttu-id="f30eb-339">Ujistěte se, že velikost hello hello nezpracovaná bitové kopie je v souladu s 1 MB.</span><span class="sxs-lookup"><span data-stu-id="f30eb-339">Make sure that hello size of hello raw image is aligned with 1 MB.</span></span> <span data-ttu-id="f30eb-340">Jinak zaokrouhlí nahoru hello velikost tooalign s 1 MB:</span><span class="sxs-lookup"><span data-stu-id="f30eb-340">Otherwise, round up hello size tooalign with 1 MB:</span></span>

        # MB=$((1024*1024))
        # size=$(qemu-img info -f raw --output json "rhel-6.8.raw" | \
          gawk 'match($0, /"virtual-size": ([0-9]+),/, val) {print val[1]}')

        # rounded_size=$((($size/$MB + 1)*$MB))
        # qemu-img resize rhel-6.8.raw $rounded_size

    <span data-ttu-id="f30eb-341">Převést tooa hello nezpracovaná disku pevné velikosti virtuálního pevného disku:</span><span class="sxs-lookup"><span data-stu-id="f30eb-341">Convert hello raw disk tooa fixed-sized VHD:</span></span>

        # qemu-img convert -f raw -o subformat=fixed -O vpc rhel-6.8.raw rhel-6.8.vhd

### <a name="prepare-a-rhel-7-virtual-machine-from-vmware"></a><span data-ttu-id="f30eb-342">Příprava virtuálního počítače, RHEL 7 z VMware</span><span class="sxs-lookup"><span data-stu-id="f30eb-342">Prepare a RHEL 7 virtual machine from VMware</span></span>
1. <span data-ttu-id="f30eb-343">Vytvořte nebo upravte hello `/etc/sysconfig/network` souboru a přidejte hello následující text:</span><span class="sxs-lookup"><span data-stu-id="f30eb-343">Create or edit hello `/etc/sysconfig/network` file, and add hello following text:</span></span>
   
        NETWORKING=yes
        HOSTNAME=localhost.localdomain

2. <span data-ttu-id="f30eb-344">Vytvořte nebo upravte hello `/etc/sysconfig/network-scripts/ifcfg-eth0` souboru a přidejte hello následující text:</span><span class="sxs-lookup"><span data-stu-id="f30eb-344">Create or edit hello `/etc/sysconfig/network-scripts/ifcfg-eth0` file, and add hello following text:</span></span>
   
        DEVICE=eth0
        ONBOOT=yes
        BOOTPROTO=dhcp
        TYPE=Ethernet
        USERCTL=no
        PEERDNS=yes
        IPV6INIT=no
        NM_CONTROLLED=no

3. <span data-ttu-id="f30eb-345">Ujistěte se, že hello síťová služba se spustí při spuštění spuštěním hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="f30eb-345">Ensure that hello network service will start at boot time by running hello following command:</span></span>

        # sudo chkconfig network on

4. <span data-ttu-id="f30eb-346">Zaregistrujte předplatné Red Hat tooenable hello instalace balíčků z úložiště RHEL hello spuštěním hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="f30eb-346">Register your Red Hat subscription tooenable hello installation of packages from hello RHEL repository by running hello following command:</span></span>

        # sudo subscription-manager register --auto-attach --username=XXX --password=XXX

5. <span data-ttu-id="f30eb-347">Upravte hello jádra spouštěcí řádku grub konfigurace tooinclude další jádra parametry pro Azure.</span><span class="sxs-lookup"><span data-stu-id="f30eb-347">Modify hello kernel boot line in your grub configuration tooinclude additional kernel parameters for Azure.</span></span> <span data-ttu-id="f30eb-348">toodo tato úprava, otevřete `/etc/default/grub` v textovém editoru a upravit hello `GRUB_CMDLINE_LINUX` parametr.</span><span class="sxs-lookup"><span data-stu-id="f30eb-348">toodo this modification, open `/etc/default/grub` in a text editor, and edit hello `GRUB_CMDLINE_LINUX` parameter.</span></span> <span data-ttu-id="f30eb-349">Například:</span><span class="sxs-lookup"><span data-stu-id="f30eb-349">For example:</span></span>
   
        GRUB_CMDLINE_LINUX="rootdelay=300 console=ttyS0 earlyprintk=ttyS0 net.ifnames=0"
   
   <span data-ttu-id="f30eb-350">Tato konfigurace taky zajišťuje, že všechny zprávy konzoly jsou odesílány toohello první sériového portu, který může být užitečné Azure podporu pro ladění problémů.</span><span class="sxs-lookup"><span data-stu-id="f30eb-350">This configuration also ensures that all console messages are sent toohello first serial port, which can assist Azure support with debugging issues.</span></span> <span data-ttu-id="f30eb-351">Je také vypne hello nové RHEL 7 zásady vytváření názvů pro síťové adaptéry.</span><span class="sxs-lookup"><span data-stu-id="f30eb-351">It also turns off hello new RHEL 7 naming conventions for NICs.</span></span> <span data-ttu-id="f30eb-352">Kromě toho doporučujeme odebrat hello následující parametry:</span><span class="sxs-lookup"><span data-stu-id="f30eb-352">In addition, we recommend that you remove hello following parameters:</span></span>
   
        rhgb quiet crashkernel=auto
   
    <span data-ttu-id="f30eb-353">Grafické a quiet spouštěcí nejsou užitečné při cloudovém prostředí, kde chceme, aby všechny protokoly toobe hello odeslané toohello sériového portu.</span><span class="sxs-lookup"><span data-stu-id="f30eb-353">Graphical and quiet boot are not useful in a cloud environment where we want all hello logs toobe sent toohello serial port.</span></span> <span data-ttu-id="f30eb-354">Můžete ponechat hello `crashkernel` možnost nakonfigurované v případě potřeby.</span><span class="sxs-lookup"><span data-stu-id="f30eb-354">You can leave hello `crashkernel` option configured if desired.</span></span> <span data-ttu-id="f30eb-355">Všimněte si, že tento parametr snižuje hello množství dostupné paměti v hello virtuálního počítače pomocí 128 MB nebo víc, což může způsobovat problémy menší velikostí virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="f30eb-355">Note that this parameter reduces hello amount of available memory in hello virtual machine by 128 MB or more, which might be problematic on smaller virtual machine sizes.</span></span>

6. <span data-ttu-id="f30eb-356">Po dokončení úprav `/etc/default/grub`spusťte hello následující příkaz toorebuild hello grub konfigurace:</span><span class="sxs-lookup"><span data-stu-id="f30eb-356">After you are done editing `/etc/default/grub`, run hello following command toorebuild hello grub configuration:</span></span>

        # sudo grub2-mkconfig -o /boot/grub2/grub.cfg

7. <span data-ttu-id="f30eb-357">Přidejte tooinitramfs modulů technologie Hyper-V.</span><span class="sxs-lookup"><span data-stu-id="f30eb-357">Add Hyper-V modules tooinitramfs.</span></span>

    <span data-ttu-id="f30eb-358">Upravit `/etc/dracut.conf`, přidejte obsah:</span><span class="sxs-lookup"><span data-stu-id="f30eb-358">Edit `/etc/dracut.conf`, add content:</span></span>

        add_drivers+="hv_vmbus hv_netvsc hv_storvsc"

    <span data-ttu-id="f30eb-359">Znovu sestavte initramfs:</span><span class="sxs-lookup"><span data-stu-id="f30eb-359">Rebuild initramfs:</span></span>

        # dracut -f -v

8. <span data-ttu-id="f30eb-360">Ujistěte se, že server SSH hello je nainstalován a nakonfigurován toostart při spuštění.</span><span class="sxs-lookup"><span data-stu-id="f30eb-360">Ensure that hello SSH server is installed and configured toostart at boot time.</span></span> <span data-ttu-id="f30eb-361">Toto nastavení je obvykle výchozí hello.</span><span class="sxs-lookup"><span data-stu-id="f30eb-361">This setting is usually hello default.</span></span> <span data-ttu-id="f30eb-362">Upravit `/etc/ssh/sshd_config` tooinclude hello následující řádek:</span><span class="sxs-lookup"><span data-stu-id="f30eb-362">Modify `/etc/ssh/sshd_config` tooinclude hello following line:</span></span>

        ClientAliveInterval 180

9. <span data-ttu-id="f30eb-363">balíček WALinuxAgent Hello `WALinuxAgent-<version>`, bylo posunuto toohello Red Hat funkce úložiště.</span><span class="sxs-lookup"><span data-stu-id="f30eb-363">hello WALinuxAgent package, `WALinuxAgent-<version>`, has been pushed toohello Red Hat extras repository.</span></span> <span data-ttu-id="f30eb-364">Povolení funkce úložiště hello spuštěním hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="f30eb-364">Enable hello extras repository by running hello following command:</span></span>

        # subscription-manager repos --enable=rhel-7-server-extras-rpms

10. <span data-ttu-id="f30eb-365">Nainstalujte hello Azure Linux Agent spuštěním hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="f30eb-365">Install hello Azure Linux Agent by running hello following command:</span></span>

        # sudo yum install WALinuxAgent

        # sudo systemctl enable waagent.service

11. <span data-ttu-id="f30eb-366">Nevytvářejte odkládacího prostoru na disku operačního systému hello.</span><span class="sxs-lookup"><span data-stu-id="f30eb-366">Do not create swap space on hello operating system disk.</span></span>

    <span data-ttu-id="f30eb-367">Hello Azure Linux Agent mohou automaticky konfigurovat odkládacího souboru pomocí disku hello místní prostředek, který je připojený toohello virtuální počítač po zřízení hello virtuálního počítače na platformě Azure.</span><span class="sxs-lookup"><span data-stu-id="f30eb-367">hello Azure Linux Agent can automatically configure swap space by using hello local resource disk that is attached toohello virtual machine after hello virtual machine is provisioned on Azure.</span></span> <span data-ttu-id="f30eb-368">Upozorňujeme, že hello místní prostředek disk je dočasný a může být vyprázdnit po deprovisioned hello virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="f30eb-368">Note that hello local resource disk is a temporary disk, and it might be emptied when hello virtual machine is deprovisioned.</span></span> <span data-ttu-id="f30eb-369">Po instalaci hello Azure Linux Agent v předchozím kroku hello se upravit hello následující parametry v `/etc/waagent.conf` odpovídajícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="f30eb-369">After you install hello Azure Linux Agent in hello previous step, modify hello following parameters in `/etc/waagent.conf` appropriately:</span></span>

        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this toowhatever you need it toobe.

12. <span data-ttu-id="f30eb-370">Pokud chcete toounregister hello předplatné, spusťte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="f30eb-370">If you want toounregister hello subscription, run hello following command:</span></span>

        # sudo subscription-manager unregister

13. <span data-ttu-id="f30eb-371">Spusťte hello následující příkazy toodeprovision hello virtuálního počítače a jeho přípravu pro zřizování na Azure:</span><span class="sxs-lookup"><span data-stu-id="f30eb-371">Run hello following commands toodeprovision hello virtual machine and prepare it for provisioning on Azure:</span></span>

        # sudo waagent -force -deprovision

        # export HISTSIZE=0

        # logout

14. <span data-ttu-id="f30eb-372">Vypnout hello virtuální počítač a proveďte převod formátu VHD toohello soubor VMDK hello.</span><span class="sxs-lookup"><span data-stu-id="f30eb-372">Shut down hello virtual machine, and convert hello VMDK file toohello VHD format.</span></span>

    <span data-ttu-id="f30eb-373">Nejprve převeďte formát tooraw hello obrázku:</span><span class="sxs-lookup"><span data-stu-id="f30eb-373">First convert hello image tooraw format:</span></span>

        # qemu-img convert -f vmdk -O raw rhel-7.3.vmdk rhel-7.3.raw

    <span data-ttu-id="f30eb-374">Ujistěte se, že velikost hello hello nezpracovaná bitové kopie je v souladu s 1 MB.</span><span class="sxs-lookup"><span data-stu-id="f30eb-374">Make sure that hello size of hello raw image is aligned with 1 MB.</span></span> <span data-ttu-id="f30eb-375">Jinak zaokrouhlí nahoru hello velikost tooalign s 1 MB:</span><span class="sxs-lookup"><span data-stu-id="f30eb-375">Otherwise, round up hello size tooalign with 1 MB:</span></span>

        # MB=$((1024*1024))
        # size=$(qemu-img info -f raw --output json "rhel-6.8.raw" | \
          gawk 'match($0, /"virtual-size": ([0-9]+),/, val) {print val[1]}')

        # rounded_size=$((($size/$MB + 1)*$MB))
        # qemu-img resize rhel-6.8.raw $rounded_size

    <span data-ttu-id="f30eb-376">Převést tooa hello nezpracovaná disku pevné velikosti virtuálního pevného disku:</span><span class="sxs-lookup"><span data-stu-id="f30eb-376">Convert hello raw disk tooa fixed-sized VHD:</span></span>

        # qemu-img convert -f raw -o subformat=fixed -O vpc rhel-7.3.raw rhel-7.3.vhd

## <a name="prepare-a-red-hat-based-virtual-machine-from-an-iso-by-using-a-kickstart-file-automatically"></a><span data-ttu-id="f30eb-377">Příprava virtuálního počítače, na základě Red Hat ze souboru ISO pomocí souboru kickstart automaticky</span><span class="sxs-lookup"><span data-stu-id="f30eb-377">Prepare a Red Hat-based virtual machine from an ISO by using a kickstart file automatically</span></span>
### <a name="prepare-a-rhel-7-virtual-machine-from-a-kickstart-file"></a><span data-ttu-id="f30eb-378">Příprava virtuálního počítače ze souboru kickstart RHEL 7</span><span class="sxs-lookup"><span data-stu-id="f30eb-378">Prepare a RHEL 7 virtual machine from a kickstart file</span></span>

1.  <span data-ttu-id="f30eb-379">Vytvořte soubor kickstart, který obsahuje následující obsah hello a uložte soubor hello.</span><span class="sxs-lookup"><span data-stu-id="f30eb-379">Create a kickstart file that includes hello following content, and save hello file.</span></span> <span data-ttu-id="f30eb-380">Podrobnosti o kickstart instalace najdete v tématu hello [Průvodce instalací Kickstart](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/7/html/Installation_Guide/chap-kickstart-installations.html).</span><span class="sxs-lookup"><span data-stu-id="f30eb-380">For details about kickstart installation, see hello [Kickstart Installation Guide](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/7/html/Installation_Guide/chap-kickstart-installations.html).</span></span>

        # Kickstart for provisioning a RHEL 7 Azure VM

        # System authorization information
          auth --enableshadow --passalgo=sha512

        # Use graphical install
        text

        # Do not run hello Setup Agent on first boot
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

        # Clear hello MBR
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

        # Power down hello machine after install
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

        # Disable hello root account
        usermod root -p '!!'

        # Configure swap in WALinuxAgent
        sed -i 's/^\(ResourceDisk\.EnableSwap\)=[Nn]$/\1=y/g' /etc/waagent.conf
        sed -i 's/^\(ResourceDisk\.SwapSizeMB\)=[0-9]*$/\1=2048/g' /etc/waagent.conf

        # Set hello cmdline
        sed -i 's/^\(GRUB_CMDLINE_LINUX\)=".*"$/\1="console=tty1 console=ttyS0 earlyprintk=ttyS0 rootdelay=300"/g' /etc/default/grub

        # Enable SSH keepalive
        sed -i 's/^#\(ClientAliveInterval\).*$/\1 180/g' /etc/ssh/sshd_config

        # Build hello grub cfg
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

2. <span data-ttu-id="f30eb-381">Umístěte soubor kickstart hello kde hello instalace systému k němu přístup.</span><span class="sxs-lookup"><span data-stu-id="f30eb-381">Place hello kickstart file where hello installation system can access it.</span></span>

3. <span data-ttu-id="f30eb-382">Ve Správci technologie Hyper-V vytvořte nový virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="f30eb-382">In Hyper-V Manager, create a new virtual machine.</span></span> <span data-ttu-id="f30eb-383">Na hello **připojit virtuální pevný Disk** vyberte **připojit virtuální pevný disk později**a hello dokončení Průvodce novým virtuálním počítačem.</span><span class="sxs-lookup"><span data-stu-id="f30eb-383">On hello **Connect Virtual Hard Disk** page, select **Attach a virtual hard disk later**, and complete hello New Virtual Machine Wizard.</span></span>

4. <span data-ttu-id="f30eb-384">Otevřete nastavení virtuálního počítače hello:</span><span class="sxs-lookup"><span data-stu-id="f30eb-384">Open hello virtual machine settings:</span></span>

    <span data-ttu-id="f30eb-385">a.</span><span class="sxs-lookup"><span data-stu-id="f30eb-385">a.</span></span>  <span data-ttu-id="f30eb-386">Připojte nový virtuální počítač toohello virtuální pevný disk.</span><span class="sxs-lookup"><span data-stu-id="f30eb-386">Attach a new virtual hard disk toohello virtual machine.</span></span> <span data-ttu-id="f30eb-387">Ujistěte se, že tooselect **formátu virtuálního pevného disku** a **pevné velikosti**.</span><span class="sxs-lookup"><span data-stu-id="f30eb-387">Make sure tooselect **VHD Format** and **Fixed Size**.</span></span>

    <span data-ttu-id="f30eb-388">b.</span><span class="sxs-lookup"><span data-stu-id="f30eb-388">b.</span></span>  <span data-ttu-id="f30eb-389">Připojte hello instalace jednotku DVD toohello ISO.</span><span class="sxs-lookup"><span data-stu-id="f30eb-389">Attach hello installation ISO toohello DVD drive.</span></span>

    <span data-ttu-id="f30eb-390">c.</span><span class="sxs-lookup"><span data-stu-id="f30eb-390">c.</span></span>  <span data-ttu-id="f30eb-391">Nastavení systému BIOS tooboot hello z disku CD-ROM.</span><span class="sxs-lookup"><span data-stu-id="f30eb-391">Set hello BIOS tooboot from CD.</span></span>

5. <span data-ttu-id="f30eb-392">Spusťte virtuální počítač hello.</span><span class="sxs-lookup"><span data-stu-id="f30eb-392">Start hello virtual machine.</span></span> <span data-ttu-id="f30eb-393">Jakmile se zobrazí Průvodce instalací hello, stiskněte klávesu **kartě** možnosti spuštění tooconfigure hello.</span><span class="sxs-lookup"><span data-stu-id="f30eb-393">When hello installation guide appears, press **Tab** tooconfigure hello boot options.</span></span>

6. <span data-ttu-id="f30eb-394">Zadejte `inst.ks=<hello location of hello kickstart file>` na konci hello hello možnosti spuštění a stiskněte klávesu **Enter**.</span><span class="sxs-lookup"><span data-stu-id="f30eb-394">Enter `inst.ks=<hello location of hello kickstart file>` at hello end of hello boot options, and press **Enter**.</span></span>

7. <span data-ttu-id="f30eb-395">Počkejte toofinish instalace hello.</span><span class="sxs-lookup"><span data-stu-id="f30eb-395">Wait for hello installation toofinish.</span></span> <span data-ttu-id="f30eb-396">Po dokončení, hello virtuální počítač se vypne automaticky.</span><span class="sxs-lookup"><span data-stu-id="f30eb-396">When it's finished, hello virtual machine will be shut down automatically.</span></span> <span data-ttu-id="f30eb-397">Svůj disk VHD Linux je nyní připraven toobe nahrán tooAzure.</span><span class="sxs-lookup"><span data-stu-id="f30eb-397">Your Linux VHD is now ready toobe uploaded tooAzure.</span></span>

## <a name="known-issues"></a><span data-ttu-id="f30eb-398">Známé problémy</span><span class="sxs-lookup"><span data-stu-id="f30eb-398">Known issues</span></span>
### <a name="hello-hyper-v-driver-could-not-be-included-in-hello-initial-ram-disk-when-using-a-non-hyper-v-hypervisor"></a><span data-ttu-id="f30eb-399">ovladač Hello technologie Hyper-V není v počáteční disku RAM hello při použití hypervisor bez technologie Hyper-V</span><span class="sxs-lookup"><span data-stu-id="f30eb-399">hello Hyper-V driver could not be included in hello initial RAM disk when using a non-Hyper-V hypervisor</span></span>

<span data-ttu-id="f30eb-400">V některých případech instalačních programů Linux nemusí zahrnovat hello ovladače pro Hyper-V v hello počáteční paměť RAM disku (initrd nebo initramfs) Pokud Linux zjistí, zda je spuštěna v prostředí Hyper-V.</span><span class="sxs-lookup"><span data-stu-id="f30eb-400">In some cases, Linux installers might not include hello drivers for Hyper-V in hello initial RAM disk (initrd or initramfs) unless Linux detects that it is running in a Hyper-V environment.</span></span>

<span data-ttu-id="f30eb-401">Pokud používáte tooprepare systému (tj. Virtualbox, Xen atd.) jiným virtualizačním bitové kopie systému Linux, možná budete muset tooensure initrd toorebuild, který alespoň hello hv_vmbus a hv_storvsc jádra moduly jsou k dispozici na disku počáteční paměť RAM hello.</span><span class="sxs-lookup"><span data-stu-id="f30eb-401">When you're using a different virtualization system (that is, Virtualbox, Xen, etc.) tooprepare your Linux image, you might need toorebuild initrd tooensure that at least hello hv_vmbus and hv_storvsc kernel modules are available on hello initial RAM disk.</span></span> <span data-ttu-id="f30eb-402">V systémech, které jsou založeny na nadřazený distribuční Red Hat hello alespoň jde o známý problém.</span><span class="sxs-lookup"><span data-stu-id="f30eb-402">This is a known issue at least on systems that are based on hello upstream Red Hat distribution.</span></span>

<span data-ttu-id="f30eb-403">tooresolve tento problém, přidejte tooinitramfs modulů technologie Hyper-V a znovu ji vytvořit:</span><span class="sxs-lookup"><span data-stu-id="f30eb-403">tooresolve this issue, add Hyper-V modules tooinitramfs and rebuild it:</span></span>

<span data-ttu-id="f30eb-404">Upravit `/etc/dracut.conf`a přidejte hello následující obsah:</span><span class="sxs-lookup"><span data-stu-id="f30eb-404">Edit `/etc/dracut.conf`, and add hello following content:</span></span>

        add_drivers+="hv_vmbus hv_netvsc hv_storvsc"

<span data-ttu-id="f30eb-405">Znovu sestavte initramfs:</span><span class="sxs-lookup"><span data-stu-id="f30eb-405">Rebuild initramfs:</span></span>

        # dracut -f -v

<span data-ttu-id="f30eb-406">Další podrobnosti najdete v tématu hello informace [znovu sestavit initramfs](https://access.redhat.com/solutions/1958).</span><span class="sxs-lookup"><span data-stu-id="f30eb-406">For more details, see hello information about [rebuilding initramfs](https://access.redhat.com/solutions/1958).</span></span>

## <a name="next-steps"></a><span data-ttu-id="f30eb-407">Další kroky</span><span class="sxs-lookup"><span data-stu-id="f30eb-407">Next steps</span></span>
<span data-ttu-id="f30eb-408">Můžete se teď připravena toouse Red Hat Enterprise Linux virtuální pevný disk toocreate nové virtuální počítače v Azure.</span><span class="sxs-lookup"><span data-stu-id="f30eb-408">You're now ready toouse your Red Hat Enterprise Linux virtual hard disk toocreate new virtual machines in Azure.</span></span> <span data-ttu-id="f30eb-409">Pokud je to hello poprvé, že jste odesílání tooAzure soubor VHD hello, najdete v části kroky 2 a 3 v [vytváření a odesílání virtuální pevný disk, který obsahuje operační systém Linux hello](classic/create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="f30eb-409">If this is hello first time that you're uploading hello .vhd file tooAzure, see steps 2 and 3 in [Creating and uploading a virtual hard disk that contains hello Linux operating system](classic/create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).</span></span>

<span data-ttu-id="f30eb-410">Další podrobnosti o hello hypervisorů, které jsou certifikované toorun Red Hat Enterprise Linux, najdete v části [hello Red Hat webu](https://access.redhat.com/certified-hypervisors).</span><span class="sxs-lookup"><span data-stu-id="f30eb-410">For more details about hello hypervisors that are certified toorun Red Hat Enterprise Linux, see [hello Red Hat website](https://access.redhat.com/certified-hypervisors).</span></span>
