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
# <a name="prepare-an-oracle-linux-virtual-machine-for-azure"></a>Příprava virtuálního počítače s Oracle Linuxem pro Azure
[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

## <a name="prerequisites"></a>Požadavky
Tento článek předpokládá, že jste již nainstalovali Oracle Linux tooa virtuální pevný disk operačního systému. Několik nástrojů existovat toocreate soubory VHD, například řešení virtualizace jako je například technologie Hyper-V. Pokyny najdete v tématu [instalace hello Role Hyper-V a konfigurace virtuálního počítače](http://technet.microsoft.com/library/hh846766.aspx).

### <a name="oracle-linux-installation-notes"></a>Poznámky k instalaci Oracle Linux
* Najdete také [obecné poznámky k instalaci Linux](create-upload-generic.md#general-linux-installation-notes) Příprava na Azure Linux další tipy pro.
* Oracle Red Hat kompatibilní jádra a jejich UEK3 (nedělitelné Enterprise jádra) jsou podporovány v Hyper-V a Azure. Nejlepších výsledků dosáhnete buďte jisti tooupdate toohello nejnovější jádra při přípravě virtuálního pevného disku vašeho Oracle Linux.
* Oracle na UEK2 se nepodporuje na Hyper-V a Azure, protože neobsahuje hello požadované ovladače.
* Formát VHDX Hello není podporován v Azure, pouze **pevný virtuální pevný disk**.  Můžete převést formát tooVHD hello disku pomocí Správce technologie Hyper-V nebo hello rutiny convert-VHD prostředí.
* Při instalaci systému Linux hello se doporučuje použít standardní oddíly spíše než LVM (často hello výchozí pro mnoho instalace). Tím se vyhnete LVM název je v konfliktu s klonovaný virtuální počítače, zvlášť pokud operační systém disk někdy potřebovat tooanother toobe připojených virtuálních počítačů pro řešení potíží. [LVM](configure-lvm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) nebo [RAID](configure-raid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) lze použít v datových disků, pokud upřednostňovaný.
* Pro větší velikosti virtuálních počítačů z důvodu chyb tooa ve verzích systému Linux jádra níže 2.6.37 nepodporuje NUMA. Toto vydání primárně ovlivňuje distribuce pomocí hello nadřazeného Red Hat 2.6.32 jádra. Ruční instalace agenta Azure Linux hello (příkaz waagent) automaticky zakáže NUMA v hello GRUB konfiguraci pro jádro Linux hello. Další informace o této naleznete v následujících kroků hello.
* Nekonfigurujte přepnutí oddílu na disku operačního systému hello. Hello Linux agent může být nakonfigurované toocreate odkládací soubor na disku hello dočasných prostředků.  Další informace o této naleznete v následujících kroků hello.
* Všechny virtuální pevné disky hello musí mít velikostí, které jsou násobky 1 MB.
* Ujistěte se, že hello `Addons` je povoleno úložiště. Upravte soubor hello `/etc/yum.repo.d/public-yum-ol6.repo`(Oracle Linux 6) nebo `/etc/yum.repo.d/public-yum-ol7.repo`(Oracle Linux) a změňte řádek hello `enabled=0` příliš`enabled=1` pod **[ol6_addons]** nebo **[ol7_addons]** v tomto soubor.

## <a name="oracle-linux-64"></a>Oracle Linux 6.4 +
Je třeba provést určitou konfiguraci kroky v hello operační systém pro virtuální počítač toorun hello v Azure.

1. V prostředním podokně hello Správce technologie Hyper-V vyberte hello virtuálního počítače.
2. Klikněte na tlačítko **připojit** tooopen hello okna pro hello virtuálního počítače.
3. Odinstalace NetworkManager spuštěním hello následující příkaz:
   
        # sudo rpm -e --nodeps NetworkManager
   
    **Poznámka:** Pokud ještě není nainstalovaný balíček hello, tento příkaz se nezdaří s chybovou zprávou. Toto chování se očekává.
4. Vytvořte soubor s názvem **sítě** v hello `/etc/sysconfig/` adresář, který obsahuje hello následující text:
   
        NETWORKING=yes
        HOSTNAME=localhost.localdomain
5. Vytvořte soubor s názvem **ifcfg eth0** v hello `/etc/sysconfig/network-scripts/` adresář, který obsahuje hello následující text:
   
        DEVICE=eth0
        ONBOOT=yes
        BOOTPROTO=dhcp
        TYPE=Ethernet
        USERCTL=no
        PEERDNS=yes
        IPV6INIT=no
6. Upravte tooavoid udev pravidla generování statická pravidla pro hello alespoň jedno rozhraní sítě Ethernet. Tato pravidla může způsobit problémy při klonování virtuálního počítače v Microsoft Azure nebo Hyper-V:
   
        # sudo ln -s /dev/null /etc/udev/rules.d/75-persistent-net-generator.rules
        # sudo rm -f /etc/udev/rules.d/70-persistent-net.rules
7. Zajistěte, aby hello síťová služba se spustí při spuštění spuštěním hello následující příkaz:
   
        # chkconfig network on
8. Instalace jazyka python pyasn1 spuštěním hello následující příkaz:
   
        # sudo yum install python-pyasn1
9. Upravte hello jádra spouštěcí řádku grub konfigurace tooinclude další jádra parametry pro Azure. toodo tomto otevřete "/ boot/grub/menu.lst" v textovém editoru a ujistěte se, že jádra výchozí hello zahrnuje hello následující parametry:
   
        console=ttyS0 earlyprintk=ttyS0 rootdelay=300 numa=off
   
   To také zajistí, všechny zprávy konzoly jsou odesílány toohello první sériového portu, který může být užitečné Azure podporu pro ladění problémů. Tato akce zakáže NUMA z důvodu chyb tooa Oracle Red Hat kompatibilní jádra systému.
   
   Kromě výše uvedených toohello, se doporučuje příliš*odebrat* hello následující parametry:
   
        rhgb quiet crashkernel=auto
   
   Grafické a quiet spouštěcí nejsou užitečné při cloudovém prostředí, kde chceme, aby všechny protokoly toobe hello odeslané toohello sériového portu.
   
   Hello `crashkernel` možnost může být vlevo nakonfigurované v případě potřeby, ale Všimněte si, že tento parametr se sníží hello množství dostupné paměti v hello virtuální počítač podle 128 MB nebo víc, což může být problematické hello menší velikostí virtuálního počítače.
10. Ujistěte se, že server SSH hello je nainstalován a nakonfigurován toostart při spuštění.  Je to obvykle výchozí hello.
11. Nainstalujte hello Azure Linux Agent tak, že spustíte následující příkaz hello. nejnovější verze Hello je 2.0.15.
    
        # sudo yum install WALinuxAgent
    
    Upozorňujeme, že dojde k instalaci balíčku WALinuxAgent hello odebrání hello NetworkManager a balíčky NetworkManager gnome Pokud nebyly již odebrána jak je popsáno v kroku 2.
12. Nevytvářejte odkládacího prostoru na disku operačního systému hello.
    
    Hello Azure Linux Agent mohou automaticky konfigurovat odkládacího souboru pomocí disku hello místní prostředek, který je připojený toohello virtuálních počítačů po zřízení v Azure. Všimněte si, je tento disk hello místní prostředek *dočasné* na disku a může být vyprázdnit po deprovisioned hello virtuálních počítačů. Po instalaci hello Azure Linux Agent (viz předchozí krok), upravte hello následující parametry v /etc/waagent.conf odpovídajícím způsobem:
    
        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this toowhatever you need it toobe.
13. Spusťte hello následující příkazy toodeprovision hello virtuálního počítače a jeho přípravu pro zřizování na Azure:
    
        # sudo waagent -force -deprovision
        # export HISTSIZE=0
        # logout
14. Klikněte na tlačítko **akce -> vypnutí dolů** ve Správci technologie Hyper-V. Svůj disk VHD Linux je nyní připraven toobe nahrán tooAzure.

- - -
## <a name="oracle-linux-70"></a>Oracle Linux 7.0 +
**Změny v systému Oracle Linux 7**

Připravuje se virtuální počítač Oracle Linux 7 pro Azure je velmi podobné tooOracle Linux 6, ale existuje několik důležitých rozdílů vhodné poznamenat:

* Hello Red Hat kompatibilní jádra a Oracle je UEK3 jsou podporovány v Azure.  doporučuje se Hello UEK3 jádra.
* Hello NetworkManager balíček už je v konfliktu s hello Azure Linux agent. Tento balíček je ve výchozím nastavení nainstalovaná a doporučujeme vám, že se neodebere.
* GRUB2 se teď používá jako výchozí zaváděcí program pro spouštění text hello, takže hello postup pro úpravy parametry jádra se změnila (viz níže).
* XFS je nyní hello výchozí systém souborů. systém souborů ext4 Hello pořád můžou použít v případě potřeby.

**Kroky konfigurace**

1. Ve Správci technologie Hyper-V vyberte hello virtuálního počítače.
2. Klikněte na tlačítko **připojit** tooopen okna konzoly pro virtuální počítač hello.
3. Vytvořte soubor s názvem **sítě** v hello `/etc/sysconfig/` adresář, který obsahuje hello následující text:
   
        NETWORKING=yes
        HOSTNAME=localhost.localdomain
4. Vytvořte soubor s názvem **ifcfg eth0** v hello `/etc/sysconfig/network-scripts/` adresář, který obsahuje hello následující text:
   
        DEVICE=eth0
        ONBOOT=yes
        BOOTPROTO=dhcp
        TYPE=Ethernet
        USERCTL=no
        PEERDNS=yes
        IPV6INIT=no
5. Upravte tooavoid udev pravidla generování statická pravidla pro hello alespoň jedno rozhraní sítě Ethernet. Tato pravidla může způsobit problémy při klonování virtuálního počítače v Microsoft Azure nebo Hyper-V:
   
        # sudo ln -s /dev/null /etc/udev/rules.d/75-persistent-net-generator.rules
6. Zajistěte, aby hello síťová služba se spustí při spuštění spuštěním hello následující příkaz:
   
        # sudo chkconfig network on
7. Instalovat balíček python pyasn1 hello spuštěním hello následující příkaz:
   
        # sudo yum install python-pyasn1
8. Spusťte následující příkaz tooclear hello aktuální yum metadata hello a nainstalujte všechny aktualizace:
   
        # sudo yum clean all
        # sudo yum -y update
9. Upravte hello jádra spouštěcí řádku grub konfigurace tooinclude další jádra parametry pro Azure. toodo tomto otevřete "/ etc/výchozí/grub" v textu editoru a úpravy hello `GRUB_CMDLINE_LINUX` parametr, například:
   
        GRUB_CMDLINE_LINUX="rootdelay=300 console=ttyS0 earlyprintk=ttyS0 net.ifnames=0"
   
   To také zajistí, všechny zprávy konzoly jsou odesílány toohello první sériového portu, který může být užitečné Azure podporu pro ladění problémů. Je také vypne hello nové OEL 7 zásady vytváření názvů pro síťové adaptéry. Kromě výše uvedených toohello, se doporučuje příliš*odebrat* hello následující parametry:
   
       rhgb quiet crashkernel=auto
   
   Grafické a quiet spouštěcí nejsou užitečné při cloudovém prostředí, kde chceme, aby všechny protokoly toobe hello odeslané toohello sériového portu.
   
   Hello `crashkernel` možnost může být vlevo nakonfigurované v případě potřeby, ale Všimněte si, že tento parametr se sníží hello množství dostupné paměti v hello virtuální počítač podle 128 MB nebo víc, což může být problematické hello menší velikostí virtuálního počítače.
10. Po dokončení úprav "/ etc/výchozí/grub" za výše, spusťte následující příkaz toorebuild hello grub konfigurace hello:
    
        # sudo grub2-mkconfig -o /boot/grub2/grub.cfg
11. Ujistěte se, že server SSH hello je nainstalován a nakonfigurován toostart při spuštění.  Je to obvykle výchozí hello.
12. Nainstalujte hello Azure Linux Agent spuštěním hello následující příkaz:
    
        # sudo yum install WALinuxAgent
        # sudo systemctl enable waagent
13. Nevytvářejte odkládacího prostoru na disku operačního systému hello.
    
    Hello Azure Linux Agent mohou automaticky konfigurovat odkládacího souboru pomocí disku hello místní prostředek, který je připojený toohello virtuálních počítačů po zřízení v Azure. Všimněte si, je tento disk hello místní prostředek *dočasné* na disku a může být vyprázdnit po deprovisioned hello virtuálních počítačů. Po instalaci hello Azure Linux Agent (viz předchozí krok hello), upravte hello následující parametry v /etc/waagent.conf odpovídajícím způsobem:
    
        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this toowhatever you need it toobe.
14. Spusťte hello následující příkazy toodeprovision hello virtuálního počítače a jeho přípravu pro zřizování na Azure:
    
        # sudo waagent -force -deprovision
        # export HISTSIZE=0
        # logout
15. Klikněte na tlačítko **akce -> vypnutí dolů** ve Správci technologie Hyper-V. Svůj disk VHD Linux je nyní připraven toobe nahrán tooAzure.

## <a name="next-steps"></a>Další kroky
Můžete se teď připravena toouse Oracle Linux VHD toocreate nové virtuální počítače v Azure. Pokud je to hello poprvé, že jste odesílání tooAzure soubor VHD hello, najdete v části kroky 2 a 3 v [vytváření a odesílání virtuální pevný disk, který obsahuje operační systém Linux hello](classic/create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).

