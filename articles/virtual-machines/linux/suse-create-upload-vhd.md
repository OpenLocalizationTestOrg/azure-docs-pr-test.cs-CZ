---
title: "aaaCreate a nahrání virtuálního pevného disku Linux SUSE v Azure"
description: "Další informace toocreate a nahrajte Azure virtuálního pevného disku (VHD) obsahující operační systém SUSE Linux."
services: virtual-machines-linux
documentationcenter: 
author: szarkos
manager: timlt
editor: tysonn
tags: azure-resource-manager,azure-service-management
ms.assetid: 066d01a6-2a54-4718-bcd0-90fe7a5303a1
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 08/24/2016
ms.author: szark
ms.openlocfilehash: 9185c7e67279357f00db0f43e944e96c58f0dd60
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="prepare-a-sles-or-opensuse-virtual-machine-for-azure"></a>Příprava virtuálního počítače se SLES nebo openSUSE pro Azure
[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

## <a name="prerequisites"></a>Požadavky
Tento článek předpokládá, že jste již nainstalovali SUSE nebo openSUSE Linux tooa virtuální pevný disk operačního systému. Několik nástrojů existovat toocreate soubory VHD, například řešení virtualizace jako je například technologie Hyper-V. Pokyny najdete v tématu [instalace hello Role Hyper-V a konfigurace virtuálního počítače](http://technet.microsoft.com/library/hh846766.aspx).

### <a name="sles--opensuse-installation-notes"></a>SLES / openSUSE poznámky k instalaci
* Najdete také [obecné poznámky k instalaci Linux](create-upload-generic.md#general-linux-installation-notes) Příprava na Azure Linux další tipy pro.
* Formát VHDX Hello není podporován v Azure, pouze **pevný virtuální pevný disk**.  Můžete převést formát tooVHD hello disku pomocí Správce technologie Hyper-V nebo hello rutiny convert-VHD prostředí.
* Při instalaci systému Linux hello se doporučuje použít standardní oddíly spíše než LVM (často hello výchozí pro mnoho instalace). Tím se vyhnete LVM název je v konfliktu s klonovaný virtuální počítače, zvlášť pokud operační systém disk někdy potřebovat tooanother toobe připojených virtuálních počítačů pro řešení potíží. [LVM](configure-lvm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) nebo [RAID](configure-raid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) lze použít v datových disků, pokud upřednostňovaný.
* Nekonfigurujte přepnutí oddílu na disku operačního systému hello. Hello Linux agent může být nakonfigurované toocreate odkládací soubor na disku hello dočasných prostředků.  Další informace o této naleznete v následujících kroků hello.
* Všechny virtuální pevné disky hello musí mít velikostí, které jsou násobky 1 MB.

## <a name="use-suse-studio"></a>Pomocí SUSE Studio
[SUSE Studio](http://www.susestudio.com) můžete snadno vytvořit a spravovat vaše SLES a openSUSE Image Azure a technologie Hyper-V. Toto je doporučená přístup pro přizpůsobení vlastní Image SLES a openSUSE hello.

Jako alternativní toobuilding vlastního virtuálního pevného disku, SUSE, publikuje také Image BYOS (přineste si vlastní předplatné) pro SLES v [VMDepot](https://vmdepot.msopentech.com/User/Show?user=1007).

## <a name="prepare-suse-linux-enterprise-server-11-sp4"></a>Příprava SUSE Linux Enterprise Server 11 SP4
1. V prostředním podokně hello Správce technologie Hyper-V vyberte hello virtuálního počítače.
2. Klikněte na tlačítko **připojit** tooopen hello okna pro hello virtuálního počítače.
3. Zaregistrovat vaše tooallow systému SUSE Linux Enterprise ho toodownload aktualizace a instalační balíčky.
4. Aktualizujte systém hello hello nejnovější opravy:
   
        # sudo zypper update
5. Hello Azure Linux Agent nainstalujte z úložiště SLES hello:
   
        # sudo zypper install WALinuxAgent
6. Zkontrolujte, pokud je příkaz waagent příliš nastavte "na" v chkconfig a pokud ne, povolit pro automatické spuštění:
   
        # sudo chkconfig waagent on
7. Zkontrolujte, zda příkaz waagent služby používá a pokud ne, spusťte ji: 
   
        # sudo service waagent start
8. Upravte hello jádra spouštěcí řádku grub konfigurace tooinclude další jádra parametry pro Azure. toodo tomto otevřete "/ boot/grub/menu.lst" v textovém editoru a ujistěte se, že jádra výchozí hello zahrnuje hello následující parametry:
   
        console=ttyS0 earlyprintk=ttyS0 rootdelay=300
   
    Tím bude zajištěno, všechny zprávy konzoly jsou odesílány toohello první sériového portu, který může být užitečné Azure podporu pro ladění problémů.
9. Potvrďte, že fstab /boot/grub/menu.lst a/etc/obou odkaz hello disku pomocí jeho UUID (podle uuid) namísto ID disku hello (podle id). 
   
    Získat UUID disku
   
        # ls /dev/disk/by-uuid/
   
    Pokud /dev/disk/by-id nebo použít, aktualizovat /boot/grub/menu.lst a/etc/fstab s hodnotou správné podle uuid hello
   
    Před změnou
   
        root=/dev/disk/by-id/SCSI-xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxxx-part1
   
    Po změně
   
        root=/dev/disk/by-uuid/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
10. Upravte tooavoid udev pravidla generování statická pravidla pro hello alespoň jedno rozhraní sítě Ethernet. Tato pravidla může způsobit problémy při klonování virtuálního počítače v Microsoft Azure nebo Hyper-V:
    
        # sudo ln -s /dev/null /etc/udev/rules.d/75-persistent-net-generator.rules
        # sudo rm -f /etc/udev/rules.d/70-persistent-net.rules
11. Je doporučeno souboru hello tooedit "/ etc a sysconfig nebo sítě nebo dhcp" a změňte hello `DHCLIENT_SET_HOSTNAME` toohello následující parametr:
    
     DHCLIENT_SET_HOSTNAME = "žádný"
12. Komentář "/ etc/sudoers", nebo odeberte hello, pokud existují následující řádky:
    
     Výchozí nastavení targetpw # požádat o heslo hello hello cílové uživatele tj kořenové všechny ALL=(ALL) všechny # upozornění! Pouze to používají společně s 'Výchozí hodnoty targetpw'!
13. Ujistěte se, že server SSH hello je nainstalován a nakonfigurován toostart při spuštění.  Je to obvykle výchozí hello.
14. Nevytvářejte odkládacího prostoru na disku operačního systému hello.
    
    Hello Azure Linux Agent mohou automaticky konfigurovat odkládacího souboru pomocí disku hello místní prostředek, který je připojený toohello virtuálních počítačů po zřízení v Azure. Všimněte si, je tento disk hello místní prostředek *dočasné* na disku a může být vyprázdnit po deprovisioned hello virtuálních počítačů. Po instalaci hello Azure Linux Agent (viz předchozí krok), upravte hello následující parametry v /etc/waagent.conf odpovídajícím způsobem:
    
     ResourceDisk.Format=y ResourceDisk.Filesystem=ext4 ResourceDisk.MountPoint=/mnt/resource ResourceDisk.EnableSwap=y ResourceDisk.SwapSizeMB=&#2048;# Poznámka: nastavte tento toowhatever potřebovat toobe.
15. Spusťte hello následující příkazy toodeprovision hello virtuálního počítače a jeho přípravu pro zřizování na Azure:
    
    # <a name="sudo-waagent--force--deprovision"></a>sudo příkaz waagent-force - deprovision
    # <a name="export-histsize0"></a>Export HISTSIZE = 0
    # <a name="logout"></a>odhlášení
16. Klikněte na tlačítko **akce -> vypnutí dolů** ve Správci technologie Hyper-V. Svůj disk VHD Linux je nyní připraven toobe nahrán tooAzure.

- - -
## <a name="prepare-opensuse-131"></a>Příprava openSUSE 13.1 +
1. V prostředním podokně hello Správce technologie Hyper-V vyberte hello virtuálního počítače.
2. Klikněte na tlačítko **připojit** tooopen hello okna pro hello virtuálního počítače.
3. V prostředí hello, spusťte příkaz hello '`zypper lr`'. Pokud tento příkaz vrátí výstup podobný toohello následující, pak hello úložiště jsou nakonfigurované podle očekávání – bez úprav jsou nezbytné (Všimněte si, že se může lišit čísla verzí):
   
        # | Alias                 | Name                  | Enabled | Refresh
        --+-----------------------+-----------------------+---------+--------
        1 | Cloud:Tools_13.1      | Cloud:Tools_13.1      | Yes     | Yes
        2 | openSUSE_13.1_OSS     | openSUSE_13.1_OSS     | Yes     | Yes
        3 | openSUSE_13.1_Updates | openSUSE_13.1_Updates | Yes     | Yes
   
    Pokud příkaz hello vrátí "Žádné úložiště definované..." použijte následující příkazy tooadd hello tyto úložiště:
   
        # sudo zypper ar -f http://download.opensuse.org/repositories/Cloud:Tools/openSUSE_13.1 Cloud:Tools_13.1
        # sudo zypper ar -f http://download.opensuse.org/distribution/13.1/repo/oss openSUSE_13.1_OSS
        # sudo zypper ar -f http://download.opensuse.org/update/13.1 openSUSE_13.1_Updates
   
    Následně můžete ověřit spuštěním příkazu hello byly přidány hello úložiště se`zypper lr`se znovu. V případě, že jeden z úložiště hello příslušné aktualizace není povolen, můžete ji povolte pomocí následujícího příkazu:
   
        # sudo zypper mr -e [NUMBER OF REPOSITORY]
4. Aktualizace hello jádra toohello nejnovější dostupnou verzi:
   
        # sudo zypper up kernel-default
   
    Nebo tooupdate hello systém s všechny nejnovější opravy hello:
   
        # sudo zypper update
5. Nainstalujte hello Azure Linux Agent.
   
   # <a name="sudo-zypper-install-walinuxagent"></a>instalace zypper sudo WALinuxAgent
6. Upravte hello jádra spouštěcí řádku grub konfigurace tooinclude další jádra parametry pro Azure. toodo tuto, otevřete "/ boot/grub/menu.lst" v textovém editoru a ujistěte se, že jádra výchozí hello zahrnuje hello následující parametry:
   
     konzole = ttyS0 earlyprintk = ttyS0 rootdelay = 300
   
   Tím bude zajištěno, všechny zprávy konzoly jsou odesílány toohello první sériového portu, který může být užitečné Azure podporu pro ladění problémů. Kromě toho odeberte hello následující parametry z řádku spouštěcí jádra hello, pokud existují:
   
     rezerva libata.atapi_enabled=0 = 0x1f0, 0x8
7. Je doporučeno souboru hello tooedit "/ etc a sysconfig nebo sítě nebo dhcp" a změňte hello `DHCLIENT_SET_HOSTNAME` toohello následující parametr:
   
     DHCLIENT_SET_HOSTNAME = "žádný"
8. **Důležité:** v "/ etc/sudoers", komentář nebo odeberte hello, pokud existují následující řádky:
   
     Výchozí nastavení targetpw # požádat o heslo hello hello cílové uživatele tj kořenové všechny ALL=(ALL) všechny # upozornění! Pouze to používají společně s 'Výchozí hodnoty targetpw'!
9. Ujistěte se, že server SSH hello je nainstalován a nakonfigurován toostart při spuštění.  Je to obvykle výchozí hello.
10. Nevytvářejte odkládacího prostoru na disku operačního systému hello.
    
    Hello Azure Linux Agent mohou automaticky konfigurovat odkládacího souboru pomocí disku hello místní prostředek, který je připojený toohello virtuálních počítačů po zřízení v Azure. Všimněte si, je tento disk hello místní prostředek *dočasné* na disku a může být vyprázdnit po deprovisioned hello virtuálních počítačů. Po instalaci hello Azure Linux Agent (viz předchozí krok), upravte hello následující parametry v /etc/waagent.conf odpovídajícím způsobem:
    
     ResourceDisk.Format=y ResourceDisk.Filesystem=ext4 ResourceDisk.MountPoint=/mnt/resource ResourceDisk.EnableSwap=y ResourceDisk.SwapSizeMB=&#2048;# Poznámka: nastavte tento toowhatever potřebovat toobe.
11. Spusťte hello následující příkazy toodeprovision hello virtuálního počítače a jeho přípravu pro zřizování na Azure:
    
    # <a name="sudo-waagent--force--deprovision"></a>sudo příkaz waagent-force - deprovision
    # <a name="export-histsize0"></a>Export HISTSIZE = 0
    # <a name="logout"></a>odhlášení
12. Ujistěte se, hello Azure Linux Agent spustí při spuštění:
    
        # sudo systemctl enable waagent.service
13. Klikněte na tlačítko **akce -> vypnutí dolů** ve Správci technologie Hyper-V. Svůj disk VHD Linux je nyní připraven toobe nahrán tooAzure.

## <a name="next-steps"></a>Další kroky
Můžete se teď připravena toouse SUSE Linux virtuální pevný disk toocreate nové virtuální počítače v Azure. Pokud je to hello poprvé, že jste odesílání tooAzure soubor VHD hello, najdete v části kroky 2 a 3 v [vytváření a odesílání virtuální pevný disk, který obsahuje operační systém Linux hello](classic/create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).

