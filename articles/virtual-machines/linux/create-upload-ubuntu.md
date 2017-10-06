---
title: "aaaCreate a nahrání Ubuntu Linux virtuálního pevného disku v Azure"
description: "Další informace toocreate a nahrajte Azure virtuálního pevného disku (VHD) obsahující Ubuntu Linux operačního systému."
services: virtual-machines-linux
documentationcenter: 
author: szarkos
manager: timlt
editor: tysonn
tags: azure-resource-manager,azure-service-management
ms.assetid: 3e097959-84fc-4f6a-8cc8-35e087fd1542
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 02/02/2017
ms.author: szark
ms.openlocfilehash: cc546a487f769b32432a7e80ddcd0f6af44e201f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="prepare-an-ubuntu-virtual-machine-for-azure"></a><span data-ttu-id="8c94e-103">Příprava virtuálního počítače s Ubuntu pro Azure</span><span class="sxs-lookup"><span data-stu-id="8c94e-103">Prepare an Ubuntu virtual machine for Azure</span></span>
[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

## <a name="official-ubuntu-cloud-images"></a><span data-ttu-id="8c94e-104">Oficiální Ubuntu cloudu obrázků</span><span class="sxs-lookup"><span data-stu-id="8c94e-104">Official Ubuntu cloud images</span></span>
<span data-ttu-id="8c94e-105">Ubuntu nyní publikuje oficiální Azure virtuální pevné disky pro stahování na [http://cloud-images.ubuntu.com/](http://cloud-images.ubuntu.com/).</span><span class="sxs-lookup"><span data-stu-id="8c94e-105">Ubuntu now publishes official Azure VHDs for download at [http://cloud-images.ubuntu.com/](http://cloud-images.ubuntu.com/).</span></span> <span data-ttu-id="8c94e-106">Pokud budete potřebovat speciální image Ubuntu toobuild pro Azure, místo použití hello Ruční postup pod ním je doporučené toostart s tyto známé funkční virtuální pevné disky a přizpůsobit podle potřeby.</span><span class="sxs-lookup"><span data-stu-id="8c94e-106">If you need toobuild your own specialized Ubuntu image for Azure, rather than use hello manual procedure below it is recommended toostart with these known working VHDs and customize as needed.</span></span> <span data-ttu-id="8c94e-107">nejnovější verze bitové kopie Hello vždy najdete na hello následující umístění:</span><span class="sxs-lookup"><span data-stu-id="8c94e-107">hello latest image releases can always be found at hello following locations:</span></span>

* <span data-ttu-id="8c94e-108">Ubuntu 12.04 nebo přesné: [ubuntu-12.04-server-cloudimg-amd64-disk1.vhd.zip](https://cloud-images.ubuntu.com/precise/current/precise-server-cloudimg-amd64-disk1.vhd.zip)</span><span class="sxs-lookup"><span data-stu-id="8c94e-108">Ubuntu 12.04/Precise: [ubuntu-12.04-server-cloudimg-amd64-disk1.vhd.zip](https://cloud-images.ubuntu.com/precise/current/precise-server-cloudimg-amd64-disk1.vhd.zip)</span></span>
* <span data-ttu-id="8c94e-109">Ubuntu 14.04/Trusty: [ubuntu-14.04-server-cloudimg-amd64-disk1.vhd.zip](http://cloud-images.ubuntu.com/releases/trusty/release/ubuntu-14.04-server-cloudimg-amd64-disk1.vhd.zip)</span><span class="sxs-lookup"><span data-stu-id="8c94e-109">Ubuntu 14.04/Trusty: [ubuntu-14.04-server-cloudimg-amd64-disk1.vhd.zip](http://cloud-images.ubuntu.com/releases/trusty/release/ubuntu-14.04-server-cloudimg-amd64-disk1.vhd.zip)</span></span>
* <span data-ttu-id="8c94e-110">Ubuntu 16.04/Xenial: [ubuntu-16.04-server-cloudimg-amd64-disk1.vhd.zip](http://cloud-images.ubuntu.com/releases/xenial/release/ubuntu-16.04-server-cloudimg-amd64-disk1.vhd.zip)</span><span class="sxs-lookup"><span data-stu-id="8c94e-110">Ubuntu 16.04/Xenial: [ubuntu-16.04-server-cloudimg-amd64-disk1.vhd.zip](http://cloud-images.ubuntu.com/releases/xenial/release/ubuntu-16.04-server-cloudimg-amd64-disk1.vhd.zip)</span></span>

## <a name="prerequisites"></a><span data-ttu-id="8c94e-111">Požadavky</span><span class="sxs-lookup"><span data-stu-id="8c94e-111">Prerequisites</span></span>
<span data-ttu-id="8c94e-112">Tento článek předpokládá, že jste již nainstalovali Ubuntu Linux tooa virtuální pevný disk operačního systému.</span><span class="sxs-lookup"><span data-stu-id="8c94e-112">This article assumes that you have already installed an Ubuntu Linux operating system tooa virtual hard disk.</span></span> <span data-ttu-id="8c94e-113">Několik nástrojů existovat toocreate soubory VHD, například řešení virtualizace jako je například technologie Hyper-V.</span><span class="sxs-lookup"><span data-stu-id="8c94e-113">Multiple tools exist toocreate .vhd files, for example a virtualization solution such as Hyper-V.</span></span> <span data-ttu-id="8c94e-114">Pokyny najdete v tématu [instalace hello Role Hyper-V a konfigurace virtuálního počítače](http://technet.microsoft.com/library/hh846766.aspx).</span><span class="sxs-lookup"><span data-stu-id="8c94e-114">For instructions, see [Install hello Hyper-V Role and Configure a Virtual Machine](http://technet.microsoft.com/library/hh846766.aspx).</span></span>

<span data-ttu-id="8c94e-115">**Poznámky k instalaci Ubuntu**</span><span class="sxs-lookup"><span data-stu-id="8c94e-115">**Ubuntu installation notes**</span></span>

* <span data-ttu-id="8c94e-116">Najdete také [obecné poznámky k instalaci Linux](create-upload-generic.md#general-linux-installation-notes) Příprava na Azure Linux další tipy pro.</span><span class="sxs-lookup"><span data-stu-id="8c94e-116">Please see also [General Linux Installation Notes](create-upload-generic.md#general-linux-installation-notes) for more tips on preparing Linux for Azure.</span></span>
* <span data-ttu-id="8c94e-117">Formát VHDX Hello není podporován v Azure, pouze **pevný virtuální pevný disk**.</span><span class="sxs-lookup"><span data-stu-id="8c94e-117">hello VHDX format is not supported in Azure, only **fixed VHD**.</span></span>  <span data-ttu-id="8c94e-118">Můžete převést formát tooVHD hello disku pomocí Správce technologie Hyper-V nebo hello rutiny convert-VHD prostředí.</span><span class="sxs-lookup"><span data-stu-id="8c94e-118">You can convert hello disk tooVHD format using Hyper-V Manager or hello convert-vhd cmdlet.</span></span>
* <span data-ttu-id="8c94e-119">Při instalaci systému Linux hello se doporučuje použít standardní oddíly spíše než LVM (často hello výchozí pro mnoho instalace).</span><span class="sxs-lookup"><span data-stu-id="8c94e-119">When installing hello Linux system it is recommended that you use standard partitions rather than LVM (often hello default for many installations).</span></span> <span data-ttu-id="8c94e-120">Tím se vyhnete LVM název je v konfliktu s klonovaný virtuální počítače, zvlášť pokud operační systém disk někdy potřebovat tooanother toobe připojených virtuálních počítačů pro řešení potíží.</span><span class="sxs-lookup"><span data-stu-id="8c94e-120">This will avoid LVM name conflicts with cloned VMs, particularly if an OS disk ever needs toobe attached tooanother VM for troubleshooting.</span></span> <span data-ttu-id="8c94e-121">[LVM](configure-lvm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) nebo [RAID](configure-raid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) lze použít v datových disků, pokud upřednostňovaný.</span><span class="sxs-lookup"><span data-stu-id="8c94e-121">[LVM](configure-lvm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) or [RAID](configure-raid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) may be used on data disks if preferred.</span></span>
* <span data-ttu-id="8c94e-122">Nekonfigurujte přepnutí oddílu na disku operačního systému hello.</span><span class="sxs-lookup"><span data-stu-id="8c94e-122">Do not configure a swap partition on hello OS disk.</span></span> <span data-ttu-id="8c94e-123">Hello Linux agent může být nakonfigurované toocreate odkládací soubor na disku hello dočasných prostředků.</span><span class="sxs-lookup"><span data-stu-id="8c94e-123">hello Linux agent can be configured toocreate a swap file on hello temporary resource disk.</span></span>  <span data-ttu-id="8c94e-124">Další informace o této naleznete v následujících kroků hello.</span><span class="sxs-lookup"><span data-stu-id="8c94e-124">More information about this can be found in hello steps below.</span></span>
* <span data-ttu-id="8c94e-125">Všechny virtuální pevné disky hello musí mít velikostí, které jsou násobky 1 MB.</span><span class="sxs-lookup"><span data-stu-id="8c94e-125">All of hello VHDs must have sizes that are multiples of 1 MB.</span></span>

## <a name="manual-steps"></a><span data-ttu-id="8c94e-126">Provedení ručních kroků</span><span class="sxs-lookup"><span data-stu-id="8c94e-126">Manual steps</span></span>
> [!NOTE]
> <span data-ttu-id="8c94e-127">Před pokusem o toocreate svoji vlastní image Ubuntu pro Azure, zvažte prosím pomocí hello předem vytvořené a testování bitových kopií z [http://cloud-images.ubuntu.com/](http://cloud-images.ubuntu.com/) místo.</span><span class="sxs-lookup"><span data-stu-id="8c94e-127">Before attempting toocreate your own custom Ubuntu image for Azure, please consider using hello pre-built and tested images from [http://cloud-images.ubuntu.com/](http://cloud-images.ubuntu.com/) instead.</span></span>
> 
> 

1. <span data-ttu-id="8c94e-128">V prostředním podokně hello Správce technologie Hyper-V vyberte hello virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="8c94e-128">In hello center pane of Hyper-V Manager, select hello virtual machine.</span></span>

2. <span data-ttu-id="8c94e-129">Klikněte na tlačítko **připojit** tooopen hello okna pro hello virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="8c94e-129">Click **Connect** tooopen hello window for hello virtual machine.</span></span>

3. <span data-ttu-id="8c94e-130">Nahraďte hello aktuální úložiště v hello image toouse Ubuntu na úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="8c94e-130">Replace hello current repositories in hello image toouse Ubuntu's Azure repos.</span></span> <span data-ttu-id="8c94e-131">kroky Hello mírně lišit v závislosti na verzi Ubuntu hello.</span><span class="sxs-lookup"><span data-stu-id="8c94e-131">hello steps vary slightly depending on hello Ubuntu version.</span></span>
   
    <span data-ttu-id="8c94e-132">Před úpravou `/etc/apt/sources.list`, je doporučeno toomake zálohy:</span><span class="sxs-lookup"><span data-stu-id="8c94e-132">Before editing `/etc/apt/sources.list`, it is recommended toomake a backup:</span></span>
   
        # sudo cp /etc/apt/sources.list /etc/apt/sources.list.bak

    <span data-ttu-id="8c94e-133">Ubuntu 12.04:</span><span class="sxs-lookup"><span data-stu-id="8c94e-133">Ubuntu 12.04:</span></span>
   
        # sudo sed -i 's/[a-z][a-z].archive.ubuntu.com/azure.archive.ubuntu.com/g' /etc/apt/sources.list
        # sudo apt-get update

    <span data-ttu-id="8c94e-134">Ubuntu 14.04:</span><span class="sxs-lookup"><span data-stu-id="8c94e-134">Ubuntu 14.04:</span></span>
   
        # sudo sed -i 's/[a-z][a-z].archive.ubuntu.com/azure.archive.ubuntu.com/g' /etc/apt/sources.list
        # sudo apt-get update

    <span data-ttu-id="8c94e-135">Ubuntu 16.04:</span><span class="sxs-lookup"><span data-stu-id="8c94e-135">Ubuntu 16.04:</span></span>
   
        # sudo sed -i 's/[a-z][a-z].archive.ubuntu.com/azure.archive.ubuntu.com/g' /etc/apt/sources.list
        # sudo apt-get update

4. <span data-ttu-id="8c94e-136">Hello Ubuntu Azure Image jsou nyní následující hello *hardwaru povolování* jádra (HWE).</span><span class="sxs-lookup"><span data-stu-id="8c94e-136">hello Ubuntu Azure images are now following hello *hardware enablement* (HWE) kernel.</span></span> <span data-ttu-id="8c94e-137">Aktualizace hello operačního systému toohello nejnovější jádra spuštěním hello následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="8c94e-137">Update hello operating system toohello latest kernel by running hello following commands:</span></span>

    <span data-ttu-id="8c94e-138">Ubuntu 12.04:</span><span class="sxs-lookup"><span data-stu-id="8c94e-138">Ubuntu 12.04:</span></span>
   
        # sudo apt-get update
        # sudo apt-get install linux-image-generic-lts-trusty linux-cloud-tools-generic-lts-trusty
        # sudo apt-get install hv-kvp-daemon-init
        (recommended) sudo apt-get dist-upgrade
   
        # sudo reboot
   
    <span data-ttu-id="8c94e-139">Ubuntu 14.04:</span><span class="sxs-lookup"><span data-stu-id="8c94e-139">Ubuntu 14.04:</span></span>
   
        # sudo apt-get update
        # sudo apt-get install linux-image-virtual-lts-vivid linux-lts-vivid-tools-common
        # sudo apt-get install hv-kvp-daemon-init
        (recommended) sudo apt-get dist-upgrade
   
        # sudo reboot

    <span data-ttu-id="8c94e-140">Ubuntu 16.04:</span><span class="sxs-lookup"><span data-stu-id="8c94e-140">Ubuntu 16.04:</span></span>
   
        # sudo apt-get update
        # sudo apt-get install linux-generic-hwe-16.04 linux-cloud-tools-generic-hwe-16.04
        (recommended) sudo apt-get dist-upgrade

        # sudo reboot

    <span data-ttu-id="8c94e-141">**Viz také:**</span><span class="sxs-lookup"><span data-stu-id="8c94e-141">**See also:**</span></span>
    - [<span data-ttu-id="8c94e-142">https://Wiki.Ubuntu.com/Kernel/LTSEnablementStack</span><span class="sxs-lookup"><span data-stu-id="8c94e-142">https://wiki.ubuntu.com/Kernel/LTSEnablementStack</span></span>](https://wiki.ubuntu.com/Kernel/LTSEnablementStack)
    - [<span data-ttu-id="8c94e-143">https://Wiki.Ubuntu.com/Kernel/RollingLTSEnablementStack</span><span class="sxs-lookup"><span data-stu-id="8c94e-143">https://wiki.ubuntu.com/Kernel/RollingLTSEnablementStack</span></span>](https://wiki.ubuntu.com/Kernel/RollingLTSEnablementStack)


5. <span data-ttu-id="8c94e-144">Upravte hello jádra spouštěcí řádek Grub tooinclude další jádra parametry pro Azure.</span><span class="sxs-lookup"><span data-stu-id="8c94e-144">Modify hello kernel boot line for Grub tooinclude additional kernel parameters for Azure.</span></span> <span data-ttu-id="8c94e-145">toodo tomto otevřete `/etc/default/grub` v textovém editoru, najít hello proměnné s názvem `GRUB_CMDLINE_LINUX_DEFAULT` (nebo v případě potřeby přidejte ho) a upravit ho tooinclude hello následující parametry:</span><span class="sxs-lookup"><span data-stu-id="8c94e-145">toodo this open `/etc/default/grub` in a text editor, find hello variable called `GRUB_CMDLINE_LINUX_DEFAULT` (or add it if needed) and edit it tooinclude hello following parameters:</span></span>
   
        GRUB_CMDLINE_LINUX_DEFAULT="console=tty1 console=ttyS0,115200n8 earlyprintk=ttyS0,115200 rootdelay=300"

    <span data-ttu-id="8c94e-146">Uložte a zavřete tento soubor a pak spusťte `sudo update-grub`.</span><span class="sxs-lookup"><span data-stu-id="8c94e-146">Save and close this file, and then run `sudo update-grub`.</span></span> <span data-ttu-id="8c94e-147">Tím bude zajištěno, že všechny zprávy konzoly jsou odesílány toohello první sériového portu, který může být užitečné Azure technickou podporu pro ladění problémů.</span><span class="sxs-lookup"><span data-stu-id="8c94e-147">This will ensure all console messages are sent toohello first serial port, which can assist Azure technical support with debugging issues.</span></span>

6. <span data-ttu-id="8c94e-148">Ujistěte se, že server SSH hello je nainstalován a nakonfigurován toostart při spuštění.</span><span class="sxs-lookup"><span data-stu-id="8c94e-148">Ensure that hello SSH server is installed and configured toostart at boot time.</span></span>  <span data-ttu-id="8c94e-149">Je to obvykle výchozí hello.</span><span class="sxs-lookup"><span data-stu-id="8c94e-149">This is usually hello default.</span></span>

7. <span data-ttu-id="8c94e-150">Nainstalujte hello Azure Linux Agent:</span><span class="sxs-lookup"><span data-stu-id="8c94e-150">Install hello Azure Linux Agent:</span></span>
   
        # sudo apt-get update
        # sudo apt-get install walinuxagent

    >[!Note]
    <span data-ttu-id="8c94e-151">Hello `walinuxagent` balíček může odebrat hello `NetworkManager` a `NetworkManager-gnome` balíčky, pokud jsou nainstalovány.</span><span class="sxs-lookup"><span data-stu-id="8c94e-151">hello `walinuxagent` package may remove hello `NetworkManager` and `NetworkManager-gnome` packages, if they are installed.</span></span>

8. <span data-ttu-id="8c94e-152">Spusťte hello následující příkazy toodeprovision hello virtuálního počítače a jeho přípravu pro zřizování na Azure:</span><span class="sxs-lookup"><span data-stu-id="8c94e-152">Run hello following commands toodeprovision hello virtual machine and prepare it for provisioning on Azure:</span></span>
   
        # sudo waagent -force -deprovision
        # export HISTSIZE=0
        # logout

9. <span data-ttu-id="8c94e-153">Klikněte na tlačítko **akce -> vypnutí dolů** ve Správci technologie Hyper-V.</span><span class="sxs-lookup"><span data-stu-id="8c94e-153">Click **Action -> Shut Down** in Hyper-V Manager.</span></span> <span data-ttu-id="8c94e-154">Svůj disk VHD Linux je nyní připraven toobe nahrán tooAzure.</span><span class="sxs-lookup"><span data-stu-id="8c94e-154">Your Linux VHD is now ready toobe uploaded tooAzure.</span></span>

## <a name="next-steps"></a><span data-ttu-id="8c94e-155">Další kroky</span><span class="sxs-lookup"><span data-stu-id="8c94e-155">Next steps</span></span>
<span data-ttu-id="8c94e-156">Můžete se teď připravena toouse Ubuntu Linux virtuální pevný disk toocreate nové virtuální počítače v Azure.</span><span class="sxs-lookup"><span data-stu-id="8c94e-156">You're now ready toouse your Ubuntu Linux virtual hard disk toocreate new virtual machines in Azure.</span></span> <span data-ttu-id="8c94e-157">Pokud je to hello poprvé, že jste odesílání tooAzure soubor VHD hello, najdete v části kroky 2 a 3 v [vytváření a odesílání virtuální pevný disk, který obsahuje operační systém Linux hello](classic/create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="8c94e-157">If this is hello first time that you're uploading hello .vhd file tooAzure, see steps 2 and 3 in [Creating and uploading a virtual hard disk that contains hello Linux operating system](classic/create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).</span></span>

## <a name="references"></a><span data-ttu-id="8c94e-158">Odkazy</span><span class="sxs-lookup"><span data-stu-id="8c94e-158">References</span></span>
<span data-ttu-id="8c94e-159">Ubuntu hardwaru povolování (HWE) jádra:</span><span class="sxs-lookup"><span data-stu-id="8c94e-159">Ubuntu hardware enablement (HWE) kernel:</span></span>

* [<span data-ttu-id="8c94e-160">http://blog.utlemming.org/2015/01/Ubuntu-1404-Azure-Images-Now-Tracking.HTML</span><span class="sxs-lookup"><span data-stu-id="8c94e-160">http://blog.utlemming.org/2015/01/ubuntu-1404-azure-images-now-tracking.html</span></span>](http://blog.utlemming.org/2015/01/ubuntu-1404-azure-images-now-tracking.html)
* [<span data-ttu-id="8c94e-161">http://blog.utlemming.org/2015/02/1204-Azure-Cloud-Images-Now-Using-hwe.HTML</span><span class="sxs-lookup"><span data-stu-id="8c94e-161">http://blog.utlemming.org/2015/02/1204-azure-cloud-images-now-using-hwe.html</span></span>](http://blog.utlemming.org/2015/02/1204-azure-cloud-images-now-using-hwe.html)

