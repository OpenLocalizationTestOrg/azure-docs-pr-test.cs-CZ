---
title: "Příprava Debian Linux virtuální pevný disk v Azure | Microsoft Docs"
description: "Naučte se vytvářet Debian 7 a 8 soubory virtuálního pevného disku pro nasazení v Azure."
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
ms.openlocfilehash: 63970d162c12984d6476bf0b9fc4ab70160eccdb
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="prepare-a-debian-vhd-for-azure"></a><span data-ttu-id="6bd5a-103">Příprava virtuálního pevného disku Debian pro Azure</span><span class="sxs-lookup"><span data-stu-id="6bd5a-103">Prepare a Debian VHD for Azure</span></span>
## <a name="prerequisites"></a><span data-ttu-id="6bd5a-104">Požadavky</span><span class="sxs-lookup"><span data-stu-id="6bd5a-104">Prerequisites</span></span>
<span data-ttu-id="6bd5a-105">Této části se předpokládá, že po instalaci operačního systému Debian Linux ze souboru .iso stáhnout z [Debian webu](https://www.debian.org/distrib/) na virtuální pevný disk.</span><span class="sxs-lookup"><span data-stu-id="6bd5a-105">This section assumes that you have already installed a Debian Linux operating system from an .iso file downloaded from the [Debian website](https://www.debian.org/distrib/) to a virtual hard disk.</span></span> <span data-ttu-id="6bd5a-106">Existuje několik nástrojů k vytvoření soubory VHD; Technologie Hyper-V je pouze jedním z příkladů.</span><span class="sxs-lookup"><span data-stu-id="6bd5a-106">Multiple tools exist to create .vhd files; Hyper-V is only one example.</span></span> <span data-ttu-id="6bd5a-107">Pokyny k použití technologie Hyper-V najdete v tématu [nainstalovat roli Hyper-V a konfigurace virtuálního počítače](https://technet.microsoft.com/library/hh846766.aspx).</span><span class="sxs-lookup"><span data-stu-id="6bd5a-107">For instructions using Hyper-V, see [Install the Hyper-V Role and Configure a Virtual Machine](https://technet.microsoft.com/library/hh846766.aspx).</span></span>

## <a name="installation-notes"></a><span data-ttu-id="6bd5a-108">Poznámky k instalaci</span><span class="sxs-lookup"><span data-stu-id="6bd5a-108">Installation notes</span></span>
* <span data-ttu-id="6bd5a-109">Najdete také [obecné poznámky k instalaci Linux](create-upload-generic.md#general-linux-installation-notes) Příprava na Azure Linux další tipy pro.</span><span class="sxs-lookup"><span data-stu-id="6bd5a-109">Please see also [General Linux Installation Notes](create-upload-generic.md#general-linux-installation-notes) for more tips on preparing Linux for Azure.</span></span>
* <span data-ttu-id="6bd5a-110">Novější formát VHDX není podporovaný v Azure.</span><span class="sxs-lookup"><span data-stu-id="6bd5a-110">The newer VHDX format is not supported in Azure.</span></span> <span data-ttu-id="6bd5a-111">Disk můžete převést do formátu virtuálního pevného disku pomocí Správce technologie Hyper-V nebo **convert-VHD prostředí** rutiny.</span><span class="sxs-lookup"><span data-stu-id="6bd5a-111">You can convert the disk to VHD format using Hyper-V Manager or the **convert-vhd** cmdlet.</span></span>
* <span data-ttu-id="6bd5a-112">Při instalaci systému Linux se doporučuje použít standardní oddíly spíše než LVM (často výchozí pro mnoho instalace).</span><span class="sxs-lookup"><span data-stu-id="6bd5a-112">When installing the Linux system it is recommended that you use standard partitions rather than LVM (often the default for many installations).</span></span> <span data-ttu-id="6bd5a-113">Tím se vyhnete LVM název je v konfliktu s klonovaný virtuální počítače, zvlášť pokud někdy musí být připojené k jiným virtuálním Počítačem pro řešení potíží s disk s operačním systémem.</span><span class="sxs-lookup"><span data-stu-id="6bd5a-113">This will avoid LVM name conflicts with cloned VMs, particularly if an OS disk ever needs to be attached to another VM for troubleshooting.</span></span> <span data-ttu-id="6bd5a-114">[LVM](configure-lvm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) nebo [RAID](configure-raid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) lze použít v datových disků, pokud upřednostňovaný.</span><span class="sxs-lookup"><span data-stu-id="6bd5a-114">[LVM](configure-lvm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) or [RAID](configure-raid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) may be used on data disks if preferred.</span></span>
* <span data-ttu-id="6bd5a-115">Nekonfigurujte přepnutí oddílu na disku operačního systému.</span><span class="sxs-lookup"><span data-stu-id="6bd5a-115">Do not configure a swap partition on the OS disk.</span></span> <span data-ttu-id="6bd5a-116">Chcete-li vytvořit odkládací soubor na disku dočasných prostředků lze nakonfigurovat Azure Linux agent.</span><span class="sxs-lookup"><span data-stu-id="6bd5a-116">The Azure Linux agent can be configured to create a swap file on the temporary resource disk.</span></span> <span data-ttu-id="6bd5a-117">Další informace o této naleznete v následujících krocích.</span><span class="sxs-lookup"><span data-stu-id="6bd5a-117">More information about this can be found in the steps below.</span></span>
* <span data-ttu-id="6bd5a-118">Všechny virtuální pevné disky musí mít velikostí, které jsou násobky 1 MB.</span><span class="sxs-lookup"><span data-stu-id="6bd5a-118">All of the VHDs must have sizes that are multiples of 1 MB.</span></span>

## <a name="use-azure-manage-to-create-debian-vhds"></a><span data-ttu-id="6bd5a-119">Použít správu Azure k vytvoření Debian virtuálních pevných disků</span><span class="sxs-lookup"><span data-stu-id="6bd5a-119">Use Azure-Manage to create Debian VHDs</span></span>
<span data-ttu-id="6bd5a-120">Nejsou k dispozici pro generování Debian virtuální pevné disky pro Azure, jako například nástroje [azure-spravovat](https://github.com/credativ/azure-manage) skriptů z [credativ](http://www.credativ.com/).</span><span class="sxs-lookup"><span data-stu-id="6bd5a-120">There are tools available for generating Debian VHDs for Azure, such as the [azure-manage](https://github.com/credativ/azure-manage) scripts from [credativ](http://www.credativ.com/).</span></span> <span data-ttu-id="6bd5a-121">Toto je doporučený postup a vytvoření bitové kopie od začátku.</span><span class="sxs-lookup"><span data-stu-id="6bd5a-121">This is the recommended approach versus creating an image from scratch.</span></span> <span data-ttu-id="6bd5a-122">Můžete například vytvořit Debian virtuální pevný disk 8 spusťte následující příkazy ke stažení azure a spravovat (a závislosti) a spusťte skript azure_build_image:</span><span class="sxs-lookup"><span data-stu-id="6bd5a-122">For example, to create a Debian 8 VHD run the following commands to download azure-manage (and dependencies) and run the azure_build_image script:</span></span>

    # sudo apt-get update
    # sudo apt-get install git qemu-utils mbr kpartx debootstrap

    # sudo apt-get install python3-pip python3-dateutil python3-cryptography
    # sudo pip3 install azure-storage azure-servicemanagement-legacy azure-common pytest pyyaml
    # git clone https://github.com/credativ/azure-manage.git
    # cd azure-manage
    # sudo pip3 install .

    # sudo azure_build_image --option release=jessie --option image_size_gb=30 --option image_prefix=debian-jessie-azure section


## <a name="manually-prepare-a-debian-vhd"></a><span data-ttu-id="6bd5a-123">Ruční Příprava Debian virtuálního pevného disku</span><span class="sxs-lookup"><span data-stu-id="6bd5a-123">Manually prepare a Debian VHD</span></span>
1. <span data-ttu-id="6bd5a-124">Ve Správci technologie Hyper-V vyberte virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="6bd5a-124">In Hyper-V Manager, select the virtual machine.</span></span>
2. <span data-ttu-id="6bd5a-125">Klikněte na tlačítko **Connect** k otevření okna konzoly pro virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="6bd5a-125">Click **Connect** to open a console window for the virtual machine.</span></span>
3. <span data-ttu-id="6bd5a-126">Komentář čáry použité k **cdrom bázi deb** v `/etc/apt/source.list` Pokud nastavíte virtuální počítač pro soubor ISO.</span><span class="sxs-lookup"><span data-stu-id="6bd5a-126">Comment out the line for **deb cdrom** in `/etc/apt/source.list` if you set up the VM against an ISO file.</span></span>
4. <span data-ttu-id="6bd5a-127">Upravit `/etc/default/grub` soubor a upravte **GRUB_CMDLINE_LINUX** parametr takto pro zahrnutí dalších jádra parametry Azure.</span><span class="sxs-lookup"><span data-stu-id="6bd5a-127">Edit the `/etc/default/grub` file and modify the **GRUB_CMDLINE_LINUX** parameter as follows to include additional kernel parameters for Azure.</span></span>
   
        GRUB_CMDLINE_LINUX="console=tty0 console=ttyS0,115200 earlyprintk=ttyS0,115200 rootdelay=30"
5. <span data-ttu-id="6bd5a-128">Grub znovu sestavte a spusťte:</span><span class="sxs-lookup"><span data-stu-id="6bd5a-128">Rebuild the grub and run:</span></span>
   
        # sudo update-grub
6. <span data-ttu-id="6bd5a-129">Přidejte úložiště Azure pro Debian /etc/apt/sources.list Debian 7 nebo 8:</span><span class="sxs-lookup"><span data-stu-id="6bd5a-129">Add Debian's Azure repositories to /etc/apt/sources.list for either Debian 7 or 8:</span></span>
   
    <span data-ttu-id="6bd5a-130">**Debian 7.x "Wheezy"**</span><span class="sxs-lookup"><span data-stu-id="6bd5a-130">**Debian 7.x "Wheezy"**</span></span>
   
        deb http://debian-archive.trafficmanager.net/debian wheezy-backports main
        deb-src http://debian-archive.trafficmanager.net/debian wheezy-backports main
        deb http://debian-archive.trafficmanager.net/debian-azure wheezy main
        deb-src http://debian-archive.trafficmanager.net/debian-azure wheezy main

    <span data-ttu-id="6bd5a-131">**Debian 8.x "Klára"**</span><span class="sxs-lookup"><span data-stu-id="6bd5a-131">**Debian 8.x "Jessie"**</span></span>

        deb http://debian-archive.trafficmanager.net/debian jessie-backports main
        deb-src http://debian-archive.trafficmanager.net/debian jessie-backports main
        deb http://debian-archive.trafficmanager.net/debian-azure jessie main
        deb-src http://debian-archive.trafficmanager.net/debian-azure jessie main


1. <span data-ttu-id="6bd5a-132">Nainstalujte agenta Azure Linux:</span><span class="sxs-lookup"><span data-stu-id="6bd5a-132">Install the Azure Linux Agent:</span></span>
   
        # sudo apt-get update
        # sudo apt-get install waagent
2. <span data-ttu-id="6bd5a-133">U Debian 7 se vyžaduje ke spuštění na základě 3.16 jádra z wheezy backports úložiště.</span><span class="sxs-lookup"><span data-stu-id="6bd5a-133">For Debian 7, it is required to run the 3.16-based kernel from the wheezy-backports repository.</span></span> <span data-ttu-id="6bd5a-134">Nejprve vytvořte soubor s názvem /etc/apt/preferences.d/linux.pref s tímto obsahem:</span><span class="sxs-lookup"><span data-stu-id="6bd5a-134">First create a file called /etc/apt/preferences.d/linux.pref with the following contents:</span></span>
   
        Package: linux-image-amd64 initramfs-tools
        Pin: release n=wheezy-backports
        Pin-Priority: 500
   
    <span data-ttu-id="6bd5a-135">Potom spusťte "sudo instalace výstižný get linux-image-amd64" k instalaci nové jádra.</span><span class="sxs-lookup"><span data-stu-id="6bd5a-135">Then run "sudo apt-get install linux-image-amd64" to install the new kernel.</span></span>
3. <span data-ttu-id="6bd5a-136">Zrušení zřízení virtuálního počítače a jeho přípravu pro zřizování v Azure a spusťte:</span><span class="sxs-lookup"><span data-stu-id="6bd5a-136">Deprovision the virtual machine and prepare it for provisioning on Azure and run:</span></span>
   
        # sudo waagent –force -deprovision
        # export HISTSIZE=0
        # logout
4. <span data-ttu-id="6bd5a-137">Klikněte na tlačítko **akce** -> vypnutí dolů ve Správci technologie Hyper-V.</span><span class="sxs-lookup"><span data-stu-id="6bd5a-137">Click **Action** -> Shut Down in Hyper-V Manager.</span></span> <span data-ttu-id="6bd5a-138">Svůj disk VHD Linux je nyní připravena k odeslání do Azure.</span><span class="sxs-lookup"><span data-stu-id="6bd5a-138">Your Linux VHD is now ready to be uploaded to Azure.</span></span>

## <a name="next-steps"></a><span data-ttu-id="6bd5a-139">Další kroky</span><span class="sxs-lookup"><span data-stu-id="6bd5a-139">Next steps</span></span>
<span data-ttu-id="6bd5a-140">Nyní jste připraveni použít Debian virtuální pevný disk k vytvoření nové virtuální počítače v Azure.</span><span class="sxs-lookup"><span data-stu-id="6bd5a-140">You're now ready to use your Debian virtual hard disk to create new virtual machines in Azure.</span></span> <span data-ttu-id="6bd5a-141">Pokud je poprvé, že jste nahrávání souboru VHD do Azure, najdete v části kroky 2 a 3 v [vytváření a odesílání virtuální pevný disk, který obsahuje operační systém Linux](classic/create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="6bd5a-141">If this is the first time that you're uploading the .vhd file to Azure, see steps 2 and 3 in [Creating and uploading a virtual hard disk that contains the Linux operating system](classic/create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).</span></span>

