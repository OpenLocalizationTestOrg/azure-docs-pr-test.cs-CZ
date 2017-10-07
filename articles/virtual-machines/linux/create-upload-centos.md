---
title: "aaaCreate a nahrajte na základě CentOS Linux virtuální pevný disk v Azure"
description: "Další informace toocreate a nahrajte Azure virtuálního pevného disku (VHD) obsahující operační systém na základě CentOS Linux."
services: virtual-machines-linux
documentationcenter: 
author: szarkos
manager: timlt
editor: tysonn
tags: azure-resource-manager,azure-service-management
ms.assetid: 0e518e92-e981-43f4-b12c-9cba1064c4bb
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 02/02/2017
ms.author: szark
ms.openlocfilehash: 78f36be1d36e3d44cb836c912d143f2c9a72aab5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="prepare-a-centos-based-virtual-machine-for-azure"></a>Příprava virtuálního počítače založeného na CentOS pro Azure
* [Příprava virtuálního počítače, CentOS 6.x pro Azure.](#centos-6x)
* [Příprava virtuálního počítače, CentOS 7.0 + pro Azure.](#centos-70)

[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

## <a name="prerequisites"></a>Požadavky
Tento článek předpokládá, že jste již nainstalovali CentOS (nebo podobné odvozených) operačního systému Linux tooa virtuální pevný disk. Několik nástrojů existovat toocreate soubory VHD, například řešení virtualizace jako je například technologie Hyper-V. Pokyny najdete v tématu [instalace hello Role Hyper-V a konfigurace virtuálního počítače](http://technet.microsoft.com/library/hh846766.aspx).

**Poznámky k instalaci centOS**

* Najdete také [obecné poznámky k instalaci Linux](create-upload-generic.md#general-linux-installation-notes) Příprava na Azure Linux další tipy pro.
* Formát VHDX Hello není podporován v Azure, pouze **pevný virtuální pevný disk**.  Můžete převést formát tooVHD hello disku pomocí Správce technologie Hyper-V nebo hello rutiny convert-VHD prostředí. Pokud používáte VirtualBox to znamená výběr **pevnou velikost** jako byl proti toohello výchozí přidělí dynamicky při vytváření disku hello.
* Při instalaci systému Linux hello je *doporučená* použít standardní oddíly spíše než LVM (často hello výchozí pro mnoho instalace). Tím se vyhnete LVM název je v konfliktu s klonovaný virtuální počítače, zvlášť pokud disk s operačním systémem někdy potřebuje toobe připojené tooanother identické virtuálního počítače pro řešení potíží. [LVM](configure-lvm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) nebo [RAID](configure-raid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) mohou být použity na datové disky.
* Vyžaduje se podpora jádra pro připojení systémy souborů UDF. Při prvním spuštění v Azure hello je předána zřizování konfigurace toohello virtuálního počítače s Linuxem pomocí formátu UDF média, který je připojený toohello hosta. Hello Azure Linux agent musí být schopný toomount hello UDF soubor systému tooread své konfiguraci a zřídit hello virtuálních počítačů.
* Verze jádra Linux pod 2.6.37 nepodporují NUMA v technologii Hyper-V s větší velikostí virtuálních počítačů. -Li tento problém ovlivňuje především starší distribuce pomocí hello proti proudu Red Hat 2.6.32 jádra která byla opravena v RHEL 6.6 (jádra 2.6.32 504). Systémy s operačním systémem vlastní jádra starší než 2.6.37 nebo na základě RHEL jádra starší, než se musí nastavit 2.6.32-504 hello parametr spouštěcího `numa=off` na příkazového řádku v grub.conf hello jádra. Další informace najdete v části Red Hat [KB 436883](https://access.redhat.com/solutions/436883).
* Nekonfigurujte přepnutí oddílu na disku operačního systému hello. Hello Linux agent může být nakonfigurované toocreate odkládací soubor na disku hello dočasných prostředků.  Další informace o této naleznete v následujících kroků hello.
* Všechny virtuální pevné disky hello musí mít velikostí, které jsou násobky 1 MB.

## <a name="centos-6x"></a>CentOS 6.x

1. Ve Správci technologie Hyper-V vyberte hello virtuálního počítače.

2. Klikněte na tlačítko **připojit** tooopen okna konzoly pro virtuální počítač hello.

3. V CentOS 6 NetworkManager může narušovat hello Azure Linux agent. Odinstalaci tohoto balíčku spuštěním hello následující příkaz:
   
        # sudo rpm -e --nodeps NetworkManager

4. Vytvořte nebo upravte soubor hello `/etc/sysconfig/network` a přidejte hello následující text:
   
        NETWORKING=yes
        HOSTNAME=localhost.localdomain

5. Vytvořte nebo upravte soubor hello `/etc/sysconfig/network-scripts/ifcfg-eth0` a přidejte hello následující text:
   
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
   
        # sudo chkconfig network on

8. Pokud chcete toouse hello OpenLogic zrcadlení, které jsou hostované v rámci hello datových centrech Azure, potom můžete nahradit hello `/etc/yum.repos.d/CentOS-Base.repo` soubor s hello následující úložiště.  Taky se přidá hello **[openlogic]** úložiště, který zahrnuje další balíčků, jako jsou hello Azure Linux agent:

        [openlogic]
        name=CentOS-$releasever - openlogic packages for $basearch
        baseurl=http://olcentgbl.trafficmanager.net/openlogic/$releasever/openlogic/$basearch/
        enabled=1
        gpgcheck=0
        
        [base]
        name=CentOS-$releasever - Base
        #mirrorlist=http://mirrorlist.centos.org/?release=$releasever&arch=$basearch&repo=os&infra=$infra
        baseurl=http://olcentgbl.trafficmanager.net/centos/$releasever/os/$basearch/
        gpgcheck=1
        gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-6
        
        #released updates
        [updates]
        name=CentOS-$releasever - Updates
        #mirrorlist=http://mirrorlist.centos.org/?release=$releasever&arch=$basearch&repo=updates&infra=$infra
        baseurl=http://olcentgbl.trafficmanager.net/centos/$releasever/updates/$basearch/
        gpgcheck=1
        gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-6
        
        #additional packages that may be useful
        [extras]
        name=CentOS-$releasever - Extras
        #mirrorlist=http://mirrorlist.centos.org/?release=$releasever&arch=$basearch&repo=extras&infra=$infra
        baseurl=http://olcentgbl.trafficmanager.net/centos/$releasever/extras/$basearch/
        gpgcheck=1
        gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-6

        #additional packages that extend functionality of existing packages
        [centosplus]
        name=CentOS-$releasever - Plus
        #mirrorlist=http://mirrorlist.centos.org/?release=$releasever&arch=$basearch&repo=centosplus&infra=$infra
        baseurl=http://olcentgbl.trafficmanager.net/centos/$releasever/centosplus/$basearch/
        gpgcheck=1
        enabled=0
        gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-6
        
        #contrib - packages by Centos Users
        [contrib]
        name=CentOS-$releasever - Contrib
        #mirrorlist=http://mirrorlist.centos.org/?release=$releasever&arch=$basearch&repo=contrib&infra=$infra
        baseurl=http://olcentgbl.trafficmanager.net/centos/$releasever/contrib/$basearch/
        gpgcheck=1
        enabled=0
        gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-6

    >[!Note]
    Hello zbývající části této příručky bude předpokládat, že používáte alespoň hello `[openlogic]` úložišti, který bude použité tooinstall hello Azure Linux agent níže.


9. Přidejte následující řádek too/etc/yum.conf hello:
    
        http_caching=packages

10. Spusťte následující příkaz tooclear hello aktuální yum metadata a aktualizace hello systém pomocí nejnovější balíčků hello hello:
    
        # yum clean all

    Pokud chcete vytvořit bitovou kopii pro starší verze CentOS, doporučuje se, že tooupdate, které všechny hello balíčky toohello nejnovější:

        # sudo yum -y update

    Po spuštění tohoto příkazu může být nutný restart.

11. (Volitelné) Nainstalujte hello ovladače pro hello Linux integrační služby (LIS).
   
    >[!IMPORTANT]
    Krok Hello je **požadované** pro CentOS 6.3 a starší a volitelné pro pozdějších verzích.

        # sudo rpm -e hypervkvpd  ## (may return error if not installed, that's OK)
        # sudo yum install microsoft-hyper-v

    Alternativně můžete postupujte podle pokynů pro ruční instalaci hello na hello [stránce pro stažení LIS](https://go.microsoft.com/fwlink/?linkid=403033) tooinstall hello RPM do virtuálního počítače.
 
12. Nainstalujte hello Azure Linux Agent a závislosti:
    
        # sudo yum install python-pyasn1 WALinuxAgent
    
    balíček WALinuxAgent Hello odstraní hello NetworkManager a NetworkManager gnome balíčky, pokud nebyly již odebrána popsaný v kroku 3.


13. Upravte hello jádra spouštěcí řádku grub konfigurace tooinclude další jádra parametry pro Azure. toodo tuto, otevřete `/boot/grub/menu.lst` v textovém editoru a ujistěte se, že jádra výchozí hello zahrnuje hello následující parametry:
    
        console=ttyS0 earlyprintk=ttyS0 rootdelay=300
    
    To také zajistí, všechny zprávy konzoly jsou odesílány toohello první sériového portu, který může být užitečné Azure podporu pro ladění problémů.
    
    Kromě výše uvedených toohello, se doporučuje příliš*odebrat* hello následující parametry:
    
        rhgb quiet crashkernel=auto
    
    Grafické a quiet spouštěcí nejsou užitečné při cloudovém prostředí, kde chceme, aby všechny protokoly toobe hello odeslané toohello sériového portu.  Hello `crashkernel` možnost může být vlevo nakonfigurované v případě potřeby, ale Všimněte si, že tento parametr se sníží hello množství dostupné paměti v hello virtuální počítač podle 128 MB nebo víc, což může být problematické hello menší velikostí virtuálního počítače.

    >[!Important]
    CentOS verze 6.5 a starší, musíte taky nastavit parametr jádra hello `numa=off`. V tématu Red Hat [KB 436883](https://access.redhat.com/solutions/436883).

14. Ujistěte se, že server SSH hello je nainstalován a nakonfigurován toostart při spuštění.  Je to obvykle výchozí hello.

15. Nevytvářejte odkládacího prostoru na disku operačního systému hello.
    
    Hello Azure Linux Agent mohou automaticky konfigurovat odkládacího souboru pomocí disku hello místní prostředek, který je připojený toohello virtuálních počítačů po zřízení v Azure. Všimněte si, je tento disk hello místní prostředek *dočasné* na disku a může být vyprázdnit po deprovisioned hello virtuálních počítačů. Po instalaci hello Azure Linux Agent (viz předchozí krok), upravte hello následující parametry v `/etc/waagent.conf` odpovídajícím způsobem:
    
        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this toowhatever you need it toobe.

16. Spusťte hello následující příkazy toodeprovision hello virtuálního počítače a jeho přípravu pro zřizování na Azure:
    
        # sudo waagent -force -deprovision
        # export HISTSIZE=0
        # logout

17. Klikněte na tlačítko **akce -> vypnutí dolů** ve Správci technologie Hyper-V. Svůj disk VHD Linux je nyní připraven toobe nahrán tooAzure.


- - -
## <a name="centos-70"></a>CentOS 7.0 +
**Změny v CentOS 7 (a podobné odvozené konfigurace)**

Příprava CentOS 7 virtuálního počítače na Azure je velmi podobné tooCentOS 6, ale existuje několik důležitých rozdílů vhodné poznamenat:

* Hello NetworkManager balíček už je v konfliktu s hello Azure Linux agent. Tento balíček je ve výchozím nastavení nainstalovaná a doporučujeme vám, že se neodebere.
* GRUB2 se teď používá jako výchozí zaváděcí program pro spouštění text hello, takže hello postup pro úpravy parametry jádra se změnila (viz níže).
* XFS je nyní hello výchozí systém souborů. systém souborů ext4 Hello pořád můžou použít v případě potřeby.

**Kroky konfigurace**

1. Ve Správci technologie Hyper-V vyberte hello virtuálního počítače.

2. Klikněte na tlačítko **připojit** tooopen okna konzoly pro virtuální počítač hello.

3. Vytvořte nebo upravte soubor hello `/etc/sysconfig/network` a přidejte hello následující text:
   
        NETWORKING=yes
        HOSTNAME=localhost.localdomain

4. Vytvořte nebo upravte soubor hello `/etc/sysconfig/network-scripts/ifcfg-eth0` a přidejte hello následující text:
   
        DEVICE=eth0
        ONBOOT=yes
        BOOTPROTO=dhcp
        TYPE=Ethernet
        USERCTL=no
        PEERDNS=yes
        IPV6INIT=no
        NM_CONTROLLED=no

5. Upravte tooavoid udev pravidla generování statická pravidla pro hello alespoň jedno rozhraní sítě Ethernet. Tato pravidla může způsobit problémy při klonování virtuálního počítače v Microsoft Azure nebo Hyper-V:
   
        # sudo ln -s /dev/null /etc/udev/rules.d/75-persistent-net-generator.rules

6. Pokud chcete toouse hello OpenLogic zrcadlení, které jsou hostované v rámci hello datových centrech Azure, potom můžete nahradit hello `/etc/yum.repos.d/CentOS-Base.repo` soubor s hello následující úložiště.  Taky se přidá hello **[openlogic]** úložiště, která obsahuje balíčky pro hello Azure Linux agent:
   
        [openlogic]
        name=CentOS-$releasever - openlogic packages for $basearch
        baseurl=http://olcentgbl.trafficmanager.net/openlogic/$releasever/openlogic/$basearch/
        enabled=1
        gpgcheck=0
        
        [base]
        name=CentOS-$releasever - Base
        #mirrorlist=http://mirrorlist.centos.org/?release=$releasever&arch=$basearch&repo=os&infra=$infra
        baseurl=http://olcentgbl.trafficmanager.net/centos/$releasever/os/$basearch/
        gpgcheck=1
        gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7
        
        #released updates
        [updates]
        name=CentOS-$releasever - Updates
        #mirrorlist=http://mirrorlist.centos.org/?release=$releasever&arch=$basearch&repo=updates&infra=$infra
        baseurl=http://olcentgbl.trafficmanager.net/centos/$releasever/updates/$basearch/
        gpgcheck=1
        gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7
        
        #additional packages that may be useful
        [extras]
        name=CentOS-$releasever - Extras
        #mirrorlist=http://mirrorlist.centos.org/?release=$releasever&arch=$basearch&repo=extras&infra=$infra
        baseurl=http://olcentgbl.trafficmanager.net/centos/$releasever/extras/$basearch/
        gpgcheck=1
        gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7
        
        #additional packages that extend functionality of existing packages
        [centosplus]
        name=CentOS-$releasever - Plus
        #mirrorlist=http://mirrorlist.centos.org/?release=$releasever&arch=$basearch&repo=centosplus&infra=$infra
        baseurl=http://olcentgbl.trafficmanager.net/centos/$releasever/centosplus/$basearch/
        gpgcheck=1
        enabled=0
        gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7

    >[!Note]
    Hello zbývající části této příručky bude předpokládat, že používáte alespoň hello `[openlogic]` úložišti, který bude použité tooinstall hello Azure Linux agent níže.

7. Spusťte následující příkaz tooclear hello aktuální yum metadata hello a nainstalujte všechny aktualizace:
   
        # sudo yum clean all

    Pokud chcete vytvořit bitovou kopii pro starší verze CentOS, doporučuje se, že tooupdate, které všechny hello balíčky toohello nejnovější:

        # sudo yum -y update

    Restartování, může být nutné po spuštění tohoto příkazu.

8. Upravte hello jádra spouštěcí řádku grub konfigurace tooinclude další jádra parametry pro Azure. toodo tuto, otevřete `/etc/default/grub` v textového editoru a úpravy hello `GRUB_CMDLINE_LINUX` parametr, například:
   
        GRUB_CMDLINE_LINUX="rootdelay=300 console=ttyS0 earlyprintk=ttyS0 net.ifnames=0"
   
   To také zajistí, všechny zprávy konzoly jsou odesílány toohello první sériového portu, který může být užitečné Azure podporu pro ladění problémů. Je také vypne hello nové CentOS 7 zásady vytváření názvů pro síťové adaptéry. Kromě výše uvedených toohello, se doporučuje příliš*odebrat* hello následující parametry:
   
        rhgb quiet crashkernel=auto
   
    Grafické a quiet spouštěcí nejsou užitečné při cloudovém prostředí, kde chceme, aby všechny protokoly toobe hello odeslané toohello sériového portu. Hello `crashkernel` možnost může být vlevo nakonfigurované v případě potřeby, ale Všimněte si, že tento parametr se sníží hello množství dostupné paměti v hello virtuální počítač podle 128 MB nebo víc, což může být problematické hello menší velikostí virtuálního počítače.

9. Po dokončení úprav `/etc/default/grub` za výše, spusťte následující příkaz toorebuild hello grub konfigurace hello:
   
        # sudo grub2-mkconfig -o /boot/grub2/grub.cfg

10. Pokud vytváření hello bitovou kopii z **VMWare, VirtualBox nebo KVM:** zajistěte, aby ovladače hello technologie Hyper-V jsou součástí hello initramfs:
   
   Upravit `/etc/dracut.conf`, přidejte obsah:
   
        add_drivers+=”hv_vmbus hv_netvsc hv_storvsc”
   
   Znovu sestavte hello initramfs:
   
        # sudo dracut –f -v

11. Nainstalujte hello Azure Linux Agent a závislosti:

        # sudo yum install python-pyasn1 WALinuxAgent
        # sudo systemctl enable waagent

12. Nevytvářejte odkládacího prostoru na disku operačního systému hello.
   
   Hello Azure Linux Agent mohou automaticky konfigurovat odkládacího souboru pomocí disku hello místní prostředek, který je připojený toohello virtuálních počítačů po zřízení v Azure. Všimněte si, je tento disk hello místní prostředek *dočasné* na disku a může být vyprázdnit po deprovisioned hello virtuálních počítačů. Po instalaci hello Azure Linux Agent (viz předchozí krok), upravte hello následující parametry v `/etc/waagent.conf` odpovídajícím způsobem:
   
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

## <a name="next-steps"></a>Další kroky
Můžete se teď připravena toouse systému CentOS Linux virtuální pevný disk toocreate nové virtuální počítače v Azure. Pokud je to hello poprvé, že jste odesílání tooAzure soubor VHD hello, najdete v části kroky 2 a 3 v [vytváření a odesílání virtuální pevný disk, který obsahuje operační systém Linux hello](classic/create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).

