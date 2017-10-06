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
# <a name="prepare-an-ubuntu-virtual-machine-for-azure"></a>Příprava virtuálního počítače s Ubuntu pro Azure
[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

## <a name="official-ubuntu-cloud-images"></a>Oficiální Ubuntu cloudu obrázků
Ubuntu nyní publikuje oficiální Azure virtuální pevné disky pro stahování na [http://cloud-images.ubuntu.com/](http://cloud-images.ubuntu.com/). Pokud budete potřebovat speciální image Ubuntu toobuild pro Azure, místo použití hello Ruční postup pod ním je doporučené toostart s tyto známé funkční virtuální pevné disky a přizpůsobit podle potřeby. nejnovější verze bitové kopie Hello vždy najdete na hello následující umístění:

* Ubuntu 12.04 nebo přesné: [ubuntu-12.04-server-cloudimg-amd64-disk1.vhd.zip](https://cloud-images.ubuntu.com/precise/current/precise-server-cloudimg-amd64-disk1.vhd.zip)
* Ubuntu 14.04/Trusty: [ubuntu-14.04-server-cloudimg-amd64-disk1.vhd.zip](http://cloud-images.ubuntu.com/releases/trusty/release/ubuntu-14.04-server-cloudimg-amd64-disk1.vhd.zip)
* Ubuntu 16.04/Xenial: [ubuntu-16.04-server-cloudimg-amd64-disk1.vhd.zip](http://cloud-images.ubuntu.com/releases/xenial/release/ubuntu-16.04-server-cloudimg-amd64-disk1.vhd.zip)

## <a name="prerequisites"></a>Požadavky
Tento článek předpokládá, že jste již nainstalovali Ubuntu Linux tooa virtuální pevný disk operačního systému. Několik nástrojů existovat toocreate soubory VHD, například řešení virtualizace jako je například technologie Hyper-V. Pokyny najdete v tématu [instalace hello Role Hyper-V a konfigurace virtuálního počítače](http://technet.microsoft.com/library/hh846766.aspx).

**Poznámky k instalaci Ubuntu**

* Najdete také [obecné poznámky k instalaci Linux](create-upload-generic.md#general-linux-installation-notes) Příprava na Azure Linux další tipy pro.
* Formát VHDX Hello není podporován v Azure, pouze **pevný virtuální pevný disk**.  Můžete převést formát tooVHD hello disku pomocí Správce technologie Hyper-V nebo hello rutiny convert-VHD prostředí.
* Při instalaci systému Linux hello se doporučuje použít standardní oddíly spíše než LVM (často hello výchozí pro mnoho instalace). Tím se vyhnete LVM název je v konfliktu s klonovaný virtuální počítače, zvlášť pokud operační systém disk někdy potřebovat tooanother toobe připojených virtuálních počítačů pro řešení potíží. [LVM](configure-lvm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) nebo [RAID](configure-raid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) lze použít v datových disků, pokud upřednostňovaný.
* Nekonfigurujte přepnutí oddílu na disku operačního systému hello. Hello Linux agent může být nakonfigurované toocreate odkládací soubor na disku hello dočasných prostředků.  Další informace o této naleznete v následujících kroků hello.
* Všechny virtuální pevné disky hello musí mít velikostí, které jsou násobky 1 MB.

## <a name="manual-steps"></a>Provedení ručních kroků
> [!NOTE]
> Před pokusem o toocreate svoji vlastní image Ubuntu pro Azure, zvažte prosím pomocí hello předem vytvořené a testování bitových kopií z [http://cloud-images.ubuntu.com/](http://cloud-images.ubuntu.com/) místo.
> 
> 

1. V prostředním podokně hello Správce technologie Hyper-V vyberte hello virtuálního počítače.

2. Klikněte na tlačítko **připojit** tooopen hello okna pro hello virtuálního počítače.

3. Nahraďte hello aktuální úložiště v hello image toouse Ubuntu na úložiště Azure. kroky Hello mírně lišit v závislosti na verzi Ubuntu hello.
   
    Před úpravou `/etc/apt/sources.list`, je doporučeno toomake zálohy:
   
        # sudo cp /etc/apt/sources.list /etc/apt/sources.list.bak

    Ubuntu 12.04:
   
        # sudo sed -i 's/[a-z][a-z].archive.ubuntu.com/azure.archive.ubuntu.com/g' /etc/apt/sources.list
        # sudo apt-get update

    Ubuntu 14.04:
   
        # sudo sed -i 's/[a-z][a-z].archive.ubuntu.com/azure.archive.ubuntu.com/g' /etc/apt/sources.list
        # sudo apt-get update

    Ubuntu 16.04:
   
        # sudo sed -i 's/[a-z][a-z].archive.ubuntu.com/azure.archive.ubuntu.com/g' /etc/apt/sources.list
        # sudo apt-get update

4. Hello Ubuntu Azure Image jsou nyní následující hello *hardwaru povolování* jádra (HWE). Aktualizace hello operačního systému toohello nejnovější jádra spuštěním hello následující příkazy:

    Ubuntu 12.04:
   
        # sudo apt-get update
        # sudo apt-get install linux-image-generic-lts-trusty linux-cloud-tools-generic-lts-trusty
        # sudo apt-get install hv-kvp-daemon-init
        (recommended) sudo apt-get dist-upgrade
   
        # sudo reboot
   
    Ubuntu 14.04:
   
        # sudo apt-get update
        # sudo apt-get install linux-image-virtual-lts-vivid linux-lts-vivid-tools-common
        # sudo apt-get install hv-kvp-daemon-init
        (recommended) sudo apt-get dist-upgrade
   
        # sudo reboot

    Ubuntu 16.04:
   
        # sudo apt-get update
        # sudo apt-get install linux-generic-hwe-16.04 linux-cloud-tools-generic-hwe-16.04
        (recommended) sudo apt-get dist-upgrade

        # sudo reboot

    **Viz také:**
    - [https://Wiki.Ubuntu.com/Kernel/LTSEnablementStack](https://wiki.ubuntu.com/Kernel/LTSEnablementStack)
    - [https://Wiki.Ubuntu.com/Kernel/RollingLTSEnablementStack](https://wiki.ubuntu.com/Kernel/RollingLTSEnablementStack)


5. Upravte hello jádra spouštěcí řádek Grub tooinclude další jádra parametry pro Azure. toodo tomto otevřete `/etc/default/grub` v textovém editoru, najít hello proměnné s názvem `GRUB_CMDLINE_LINUX_DEFAULT` (nebo v případě potřeby přidejte ho) a upravit ho tooinclude hello následující parametry:
   
        GRUB_CMDLINE_LINUX_DEFAULT="console=tty1 console=ttyS0,115200n8 earlyprintk=ttyS0,115200 rootdelay=300"

    Uložte a zavřete tento soubor a pak spusťte `sudo update-grub`. Tím bude zajištěno, že všechny zprávy konzoly jsou odesílány toohello první sériového portu, který může být užitečné Azure technickou podporu pro ladění problémů.

6. Ujistěte se, že server SSH hello je nainstalován a nakonfigurován toostart při spuštění.  Je to obvykle výchozí hello.

7. Nainstalujte hello Azure Linux Agent:
   
        # sudo apt-get update
        # sudo apt-get install walinuxagent

    >[!Note]
    Hello `walinuxagent` balíček může odebrat hello `NetworkManager` a `NetworkManager-gnome` balíčky, pokud jsou nainstalovány.

8. Spusťte hello následující příkazy toodeprovision hello virtuálního počítače a jeho přípravu pro zřizování na Azure:
   
        # sudo waagent -force -deprovision
        # export HISTSIZE=0
        # logout

9. Klikněte na tlačítko **akce -> vypnutí dolů** ve Správci technologie Hyper-V. Svůj disk VHD Linux je nyní připraven toobe nahrán tooAzure.

## <a name="next-steps"></a>Další kroky
Můžete se teď připravena toouse Ubuntu Linux virtuální pevný disk toocreate nové virtuální počítače v Azure. Pokud je to hello poprvé, že jste odesílání tooAzure soubor VHD hello, najdete v části kroky 2 a 3 v [vytváření a odesílání virtuální pevný disk, který obsahuje operační systém Linux hello](classic/create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).

## <a name="references"></a>Odkazy
Ubuntu hardwaru povolování (HWE) jádra:

* [http://blog.utlemming.org/2015/01/Ubuntu-1404-Azure-Images-Now-Tracking.HTML](http://blog.utlemming.org/2015/01/ubuntu-1404-azure-images-now-tracking.html)
* [http://blog.utlemming.org/2015/02/1204-Azure-Cloud-Images-Now-Using-hwe.HTML](http://blog.utlemming.org/2015/02/1204-azure-cloud-images-now-using-hwe.html)

