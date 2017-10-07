---
title: "aaaPrepare Debian systém Linux virtuálního pevného disku v Azure | Microsoft Docs"
description: "Zjistěte, jak soubory toocreate Debian 7 a 8 virtuálního pevného disku pro nasazení v Azure."
services: virtual-machines-linux
documentationcenter: 
author: szarkos
manager: timlt
editor: 
tags: azure-resource-manager,azure-service-management
ms.assetid: a6de7a7c-cc70-44e7-aed0-2ae6884d401a
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 02/02/2017
ms.author: szark
ms.openlocfilehash: e67d126de3db146357a6509aedb5f478576768f0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="prepare-a-debian-vhd-for-azure"></a><span data-ttu-id="1d353-103">Příprava virtuálního pevného disku Debian pro Azure</span><span class="sxs-lookup"><span data-stu-id="1d353-103">Prepare a Debian VHD for Azure</span></span>
## <a name="prerequisites"></a><span data-ttu-id="1d353-104">Požadavky</span><span class="sxs-lookup"><span data-stu-id="1d353-104">Prerequisites</span></span>
<span data-ttu-id="1d353-105">Této části se předpokládá, že jste již nainstalovali operačního systému Debian Linux ze souboru .iso stažený z hello [Debian webu](https://www.debian.org/distrib/) tooa virtuální pevný disk.</span><span class="sxs-lookup"><span data-stu-id="1d353-105">This section assumes that you have already installed a Debian Linux operating system from an .iso file downloaded from hello [Debian website](https://www.debian.org/distrib/) tooa virtual hard disk.</span></span> <span data-ttu-id="1d353-106">Soubory VHD toocreate; existuje několik nástrojů Technologie Hyper-V je pouze jedním z příkladů.</span><span class="sxs-lookup"><span data-stu-id="1d353-106">Multiple tools exist toocreate .vhd files; Hyper-V is only one example.</span></span> <span data-ttu-id="1d353-107">Pokyny k použití technologie Hyper-V najdete v tématu [instalace hello Role Hyper-V a konfigurace virtuálního počítače](https://technet.microsoft.com/library/hh846766.aspx).</span><span class="sxs-lookup"><span data-stu-id="1d353-107">For instructions using Hyper-V, see [Install hello Hyper-V Role and Configure a Virtual Machine](https://technet.microsoft.com/library/hh846766.aspx).</span></span>

## <a name="installation-notes"></a><span data-ttu-id="1d353-108">Poznámky k instalaci</span><span class="sxs-lookup"><span data-stu-id="1d353-108">Installation notes</span></span>
* <span data-ttu-id="1d353-109">Najdete také [obecné poznámky k instalaci Linux](create-upload-generic.md#general-linux-installation-notes) Příprava na Azure Linux další tipy pro.</span><span class="sxs-lookup"><span data-stu-id="1d353-109">Please see also [General Linux Installation Notes](create-upload-generic.md#general-linux-installation-notes) for more tips on preparing Linux for Azure.</span></span>
* <span data-ttu-id="1d353-110">Hello novější formát VHDX není podporovaný v Azure.</span><span class="sxs-lookup"><span data-stu-id="1d353-110">hello newer VHDX format is not supported in Azure.</span></span> <span data-ttu-id="1d353-111">Můžete převést na formát tooVHD hello disku pomocí Správce technologie Hyper-V nebo hello **convert-VHD prostředí** rutiny.</span><span class="sxs-lookup"><span data-stu-id="1d353-111">You can convert hello disk tooVHD format using Hyper-V Manager or hello **convert-vhd** cmdlet.</span></span>
* <span data-ttu-id="1d353-112">Při instalaci systému Linux hello se doporučuje použít standardní oddíly spíše než LVM (často hello výchozí pro mnoho instalace).</span><span class="sxs-lookup"><span data-stu-id="1d353-112">When installing hello Linux system it is recommended that you use standard partitions rather than LVM (often hello default for many installations).</span></span> <span data-ttu-id="1d353-113">Tím se vyhnete LVM název je v konfliktu s klonovaný virtuální počítače, zvlášť pokud operační systém disk někdy potřebovat tooanother toobe připojených virtuálních počítačů pro řešení potíží.</span><span class="sxs-lookup"><span data-stu-id="1d353-113">This will avoid LVM name conflicts with cloned VMs, particularly if an OS disk ever needs toobe attached tooanother VM for troubleshooting.</span></span> <span data-ttu-id="1d353-114">[LVM](configure-lvm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) nebo [RAID](configure-raid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) lze použít v datových disků, pokud upřednostňovaný.</span><span class="sxs-lookup"><span data-stu-id="1d353-114">[LVM](configure-lvm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) or [RAID](configure-raid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) may be used on data disks if preferred.</span></span>
* <span data-ttu-id="1d353-115">Nekonfigurujte přepnutí oddílu na disku operačního systému hello.</span><span class="sxs-lookup"><span data-stu-id="1d353-115">Do not configure a swap partition on hello OS disk.</span></span> <span data-ttu-id="1d353-116">Hello Azure Linux agent může být nakonfigurované toocreate odkládací soubor na disku hello dočasných prostředků.</span><span class="sxs-lookup"><span data-stu-id="1d353-116">hello Azure Linux agent can be configured toocreate a swap file on hello temporary resource disk.</span></span> <span data-ttu-id="1d353-117">Další informace o této naleznete v následujících kroků hello.</span><span class="sxs-lookup"><span data-stu-id="1d353-117">More information about this can be found in hello steps below.</span></span>
* <span data-ttu-id="1d353-118">Všechny virtuální pevné disky hello musí mít velikostí, které jsou násobky 1 MB.</span><span class="sxs-lookup"><span data-stu-id="1d353-118">All of hello VHDs must have sizes that are multiples of 1 MB.</span></span>

## <a name="use-azure-manage-toocreate-debian-vhds"></a><span data-ttu-id="1d353-119">Použití Azure spravovat toocreate Debian virtuálních pevných disků</span><span class="sxs-lookup"><span data-stu-id="1d353-119">Use Azure-Manage toocreate Debian VHDs</span></span>
<span data-ttu-id="1d353-120">Nejsou k dispozici pro generování Debian virtuální pevné disky pro Azure, jako je například hello nástroje [azure-spravovat](https://github.com/credativ/azure-manage) skriptů z [credativ](http://www.credativ.com/).</span><span class="sxs-lookup"><span data-stu-id="1d353-120">There are tools available for generating Debian VHDs for Azure, such as hello [azure-manage](https://github.com/credativ/azure-manage) scripts from [credativ](http://www.credativ.com/).</span></span> <span data-ttu-id="1d353-121">Toto je hello doporučenému přístupu a vytvoření bitové kopie od začátku.</span><span class="sxs-lookup"><span data-stu-id="1d353-121">This is hello recommended approach versus creating an image from scratch.</span></span> <span data-ttu-id="1d353-122">Například toocreate Debian 8 VHD spusťte následující příkazy spravovat azure toodownload hello (a závislosti) a spusťte skript azure_build_image hello:</span><span class="sxs-lookup"><span data-stu-id="1d353-122">For example, toocreate a Debian 8 VHD run hello following commands toodownload azure-manage (and dependencies) and run hello azure_build_image script:</span></span>

    # sudo apt-get update
    # sudo apt-get install git qemu-utils mbr kpartx debootstrap

    # sudo apt-get install python3-pip python3-dateutil python3-cryptography
    # sudo pip3 install azure-storage azure-servicemanagement-legacy azure-common pytest pyyaml
    # git clone https://github.com/credativ/azure-manage.git
    # cd azure-manage
    # sudo pip3 install .

    # sudo azure_build_image --option release=jessie --option image_size_gb=30 --option image_prefix=debian-jessie-azure section


## <a name="manually-prepare-a-debian-vhd"></a><span data-ttu-id="1d353-123">Ruční Příprava Debian virtuálního pevného disku</span><span class="sxs-lookup"><span data-stu-id="1d353-123">Manually prepare a Debian VHD</span></span>
1. <span data-ttu-id="1d353-124">Ve Správci technologie Hyper-V vyberte hello virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="1d353-124">In Hyper-V Manager, select hello virtual machine.</span></span>
2. <span data-ttu-id="1d353-125">Klikněte na tlačítko **připojit** tooopen okna konzoly pro virtuální počítač hello.</span><span class="sxs-lookup"><span data-stu-id="1d353-125">Click **Connect** tooopen a console window for hello virtual machine.</span></span>
3. <span data-ttu-id="1d353-126">Komentář hello řádek **cdrom bázi deb** v `/etc/apt/source.list` Pokud jste nastavili hello virtuálních počítačů pro soubor ISO.</span><span class="sxs-lookup"><span data-stu-id="1d353-126">Comment out hello line for **deb cdrom** in `/etc/apt/source.list` if you set up hello VM against an ISO file.</span></span>
4. <span data-ttu-id="1d353-127">Upravit hello `/etc/default/grub` soubor a upravte hello **GRUB_CMDLINE_LINUX** parametr následujícím způsobem tooinclude další jádra parametry pro Azure.</span><span class="sxs-lookup"><span data-stu-id="1d353-127">Edit hello `/etc/default/grub` file and modify hello **GRUB_CMDLINE_LINUX** parameter as follows tooinclude additional kernel parameters for Azure.</span></span>
   
        GRUB_CMDLINE_LINUX="console=tty0 console=ttyS0,115200 earlyprintk=ttyS0,115200 rootdelay=30"
5. <span data-ttu-id="1d353-128">Hello grub znovu sestavte a spusťte:</span><span class="sxs-lookup"><span data-stu-id="1d353-128">Rebuild hello grub and run:</span></span>
   
        # sudo update-grub
6. <span data-ttu-id="1d353-129">Přidejte too/etc/apt/sources.list Debian na úložiště Azure pro Debian 7 nebo 8:</span><span class="sxs-lookup"><span data-stu-id="1d353-129">Add Debian's Azure repositories too/etc/apt/sources.list for either Debian 7 or 8:</span></span>
   
    <span data-ttu-id="1d353-130">**Debian 7.x "Wheezy"**</span><span class="sxs-lookup"><span data-stu-id="1d353-130">**Debian 7.x "Wheezy"**</span></span>
   
        deb http://debian-archive.trafficmanager.net/debian wheezy-backports main
        deb-src http://debian-archive.trafficmanager.net/debian wheezy-backports main
        deb http://debian-archive.trafficmanager.net/debian-azure wheezy main
        deb-src http://debian-archive.trafficmanager.net/debian-azure wheezy main

    <span data-ttu-id="1d353-131">**Debian 8.x "Klára"**</span><span class="sxs-lookup"><span data-stu-id="1d353-131">**Debian 8.x "Jessie"**</span></span>

        deb http://debian-archive.trafficmanager.net/debian jessie-backports main
        deb-src http://debian-archive.trafficmanager.net/debian jessie-backports main
        deb http://debian-archive.trafficmanager.net/debian-azure jessie main
        deb-src http://debian-archive.trafficmanager.net/debian-azure jessie main


1. <span data-ttu-id="1d353-132">Nainstalujte hello Azure Linux Agent:</span><span class="sxs-lookup"><span data-stu-id="1d353-132">Install hello Azure Linux Agent:</span></span>
   
        # sudo apt-get update
        # sudo apt-get install waagent
2. <span data-ttu-id="1d353-133">Debian 7 je požadovaná toorun hello na základě 3.16 jádra z úložiště wheezy backports hello.</span><span class="sxs-lookup"><span data-stu-id="1d353-133">For Debian 7, it is required toorun hello 3.16-based kernel from hello wheezy-backports repository.</span></span> <span data-ttu-id="1d353-134">Nejprve vytvořte soubor s názvem /etc/apt/preferences.d/linux.pref s hello následující obsah:</span><span class="sxs-lookup"><span data-stu-id="1d353-134">First create a file called /etc/apt/preferences.d/linux.pref with hello following contents:</span></span>
   
        Package: linux-image-amd64 initramfs-tools
        Pin: release n=wheezy-backports
        Pin-Priority: 500
   
    <span data-ttu-id="1d353-135">Pak spusťte "sudo výstižný get nainstalovat linux-image-amd64" tooinstall hello nové jádra.</span><span class="sxs-lookup"><span data-stu-id="1d353-135">Then run "sudo apt-get install linux-image-amd64" tooinstall hello new kernel.</span></span>
3. <span data-ttu-id="1d353-136">Zrušení zřízení hello virtuálního počítače a jeho přípravu pro zřizování v Azure a spusťte:</span><span class="sxs-lookup"><span data-stu-id="1d353-136">Deprovision hello virtual machine and prepare it for provisioning on Azure and run:</span></span>
   
        # sudo waagent –force -deprovision
        # export HISTSIZE=0
        # logout
4. <span data-ttu-id="1d353-137">Klikněte na tlačítko **akce** -> vypnutí dolů ve Správci technologie Hyper-V.</span><span class="sxs-lookup"><span data-stu-id="1d353-137">Click **Action** -> Shut Down in Hyper-V Manager.</span></span> <span data-ttu-id="1d353-138">Svůj disk VHD Linux je nyní připraven toobe nahrán tooAzure.</span><span class="sxs-lookup"><span data-stu-id="1d353-138">Your Linux VHD is now ready toobe uploaded tooAzure.</span></span>

## <a name="next-steps"></a><span data-ttu-id="1d353-139">Další kroky</span><span class="sxs-lookup"><span data-stu-id="1d353-139">Next steps</span></span>
<span data-ttu-id="1d353-140">Můžete se teď připravena toouse Debian virtuální pevný disk toocreate nové virtuální počítače v Azure.</span><span class="sxs-lookup"><span data-stu-id="1d353-140">You're now ready toouse your Debian virtual hard disk toocreate new virtual machines in Azure.</span></span> <span data-ttu-id="1d353-141">Pokud je to hello poprvé, že jste odesílání tooAzure soubor VHD hello, najdete v části kroky 2 a 3 v [vytváření a odesílání virtuální pevný disk, který obsahuje operační systém Linux hello](classic/create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="1d353-141">If this is hello first time that you're uploading hello .vhd file tooAzure, see steps 2 and 3 in [Creating and uploading a virtual hard disk that contains hello Linux operating system](classic/create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).</span></span>

