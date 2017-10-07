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
# <a name="prepare-a-debian-vhd-for-azure"></a>Příprava virtuálního pevného disku Debian pro Azure
## <a name="prerequisites"></a>Požadavky
Této části se předpokládá, že jste již nainstalovali operačního systému Debian Linux ze souboru .iso stažený z hello [Debian webu](https://www.debian.org/distrib/) tooa virtuální pevný disk. Soubory VHD toocreate; existuje několik nástrojů Technologie Hyper-V je pouze jedním z příkladů. Pokyny k použití technologie Hyper-V najdete v tématu [instalace hello Role Hyper-V a konfigurace virtuálního počítače](https://technet.microsoft.com/library/hh846766.aspx).

## <a name="installation-notes"></a>Poznámky k instalaci
* Najdete také [obecné poznámky k instalaci Linux](create-upload-generic.md#general-linux-installation-notes) Příprava na Azure Linux další tipy pro.
* Hello novější formát VHDX není podporovaný v Azure. Můžete převést na formát tooVHD hello disku pomocí Správce technologie Hyper-V nebo hello **convert-VHD prostředí** rutiny.
* Při instalaci systému Linux hello se doporučuje použít standardní oddíly spíše než LVM (často hello výchozí pro mnoho instalace). Tím se vyhnete LVM název je v konfliktu s klonovaný virtuální počítače, zvlášť pokud operační systém disk někdy potřebovat tooanother toobe připojených virtuálních počítačů pro řešení potíží. [LVM](configure-lvm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) nebo [RAID](configure-raid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) lze použít v datových disků, pokud upřednostňovaný.
* Nekonfigurujte přepnutí oddílu na disku operačního systému hello. Hello Azure Linux agent může být nakonfigurované toocreate odkládací soubor na disku hello dočasných prostředků. Další informace o této naleznete v následujících kroků hello.
* Všechny virtuální pevné disky hello musí mít velikostí, které jsou násobky 1 MB.

## <a name="use-azure-manage-toocreate-debian-vhds"></a>Použití Azure spravovat toocreate Debian virtuálních pevných disků
Nejsou k dispozici pro generování Debian virtuální pevné disky pro Azure, jako je například hello nástroje [azure-spravovat](https://github.com/credativ/azure-manage) skriptů z [credativ](http://www.credativ.com/). Toto je hello doporučenému přístupu a vytvoření bitové kopie od začátku. Například toocreate Debian 8 VHD spusťte následující příkazy spravovat azure toodownload hello (a závislosti) a spusťte skript azure_build_image hello:

    # sudo apt-get update
    # sudo apt-get install git qemu-utils mbr kpartx debootstrap

    # sudo apt-get install python3-pip python3-dateutil python3-cryptography
    # sudo pip3 install azure-storage azure-servicemanagement-legacy azure-common pytest pyyaml
    # git clone https://github.com/credativ/azure-manage.git
    # cd azure-manage
    # sudo pip3 install .

    # sudo azure_build_image --option release=jessie --option image_size_gb=30 --option image_prefix=debian-jessie-azure section


## <a name="manually-prepare-a-debian-vhd"></a>Ruční Příprava Debian virtuálního pevného disku
1. Ve Správci technologie Hyper-V vyberte hello virtuálního počítače.
2. Klikněte na tlačítko **připojit** tooopen okna konzoly pro virtuální počítač hello.
3. Komentář hello řádek **cdrom bázi deb** v `/etc/apt/source.list` Pokud jste nastavili hello virtuálních počítačů pro soubor ISO.
4. Upravit hello `/etc/default/grub` soubor a upravte hello **GRUB_CMDLINE_LINUX** parametr následujícím způsobem tooinclude další jádra parametry pro Azure.
   
        GRUB_CMDLINE_LINUX="console=tty0 console=ttyS0,115200 earlyprintk=ttyS0,115200 rootdelay=30"
5. Hello grub znovu sestavte a spusťte:
   
        # sudo update-grub
6. Přidejte too/etc/apt/sources.list Debian na úložiště Azure pro Debian 7 nebo 8:
   
    **Debian 7.x "Wheezy"**
   
        deb http://debian-archive.trafficmanager.net/debian wheezy-backports main
        deb-src http://debian-archive.trafficmanager.net/debian wheezy-backports main
        deb http://debian-archive.trafficmanager.net/debian-azure wheezy main
        deb-src http://debian-archive.trafficmanager.net/debian-azure wheezy main

    **Debian 8.x "Klára"**

        deb http://debian-archive.trafficmanager.net/debian jessie-backports main
        deb-src http://debian-archive.trafficmanager.net/debian jessie-backports main
        deb http://debian-archive.trafficmanager.net/debian-azure jessie main
        deb-src http://debian-archive.trafficmanager.net/debian-azure jessie main


1. Nainstalujte hello Azure Linux Agent:
   
        # sudo apt-get update
        # sudo apt-get install waagent
2. Debian 7 je požadovaná toorun hello na základě 3.16 jádra z úložiště wheezy backports hello. Nejprve vytvořte soubor s názvem /etc/apt/preferences.d/linux.pref s hello následující obsah:
   
        Package: linux-image-amd64 initramfs-tools
        Pin: release n=wheezy-backports
        Pin-Priority: 500
   
    Pak spusťte "sudo výstižný get nainstalovat linux-image-amd64" tooinstall hello nové jádra.
3. Zrušení zřízení hello virtuálního počítače a jeho přípravu pro zřizování v Azure a spusťte:
   
        # sudo waagent –force -deprovision
        # export HISTSIZE=0
        # logout
4. Klikněte na tlačítko **akce** -> vypnutí dolů ve Správci technologie Hyper-V. Svůj disk VHD Linux je nyní připraven toobe nahrán tooAzure.

## <a name="next-steps"></a>Další kroky
Můžete se teď připravena toouse Debian virtuální pevný disk toocreate nové virtuální počítače v Azure. Pokud je to hello poprvé, že jste odesílání tooAzure soubor VHD hello, najdete v části kroky 2 a 3 v [vytváření a odesílání virtuální pevný disk, který obsahuje operační systém Linux hello](classic/create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).

