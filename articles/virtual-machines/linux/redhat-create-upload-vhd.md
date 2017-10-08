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
# <a name="prepare-a-red-hat-based-virtual-machine-for-azure"></a>Příprava virtuálního počítače založeného na Red Hat pro Azure
V tomto článku se dozvíte, jak tooprepare virtuální počítač Red Hat Enterprise Linux (RHEL) pro použití v Azure. Hello verze RHEL, které jsou popsané v tomto článku jsou 6.7 + a 7.1 +. Hello hypervisory pro přípravu, které jsou popsané v tomto článku jsou virtuální počítače Hyper-V, na základě jádra (KVM) a VMware. Další informace o požadavcích na podmínky pro účasti v programu Red Hat přístup ke cloudu najdete v tématu [webu přístup do cloudu Red Hat](http://www.redhat.com/en/technologies/cloud-computing/cloud-access) a [systémem RHEL v Azure](https://access.redhat.com/ecosystem/ccsp/microsoft-azure).

## <a name="prepare-a-red-hat-based-virtual-machine-from-hyper-v-manager"></a>Příprava virtuálního počítače, na základě Red Hat ze Správce technologie Hyper-V

### <a name="prerequisites"></a>Požadavky
V této části se předpokládá, že jste již získali soubor ISO od hello Red Hat webu a nainstalované hello RHEL image tooa virtuální pevný disk (VHD). Další informace o toouse Správce technologie Hyper-V tooinstall image operačního systému najdete v části [instalace hello Role Hyper-V a konfigurace virtuálního počítače](http://technet.microsoft.com/library/hh846766.aspx).

**Poznámky k instalaci RHEL**

* Azure nepodporuje hello formátu VHDX. Azure podporuje pouze dlouhodobý virtuálního pevného disku. Můžete použít formát tooVHD disku hello tooconvert Správce technologie Hyper-V, nebo můžete pomocí rutiny convert-VHD prostředí hello. Pokud používáte VirtualBox, vyberte **pevnou velikost** názvem na rozdíl od toohello výchozí možnost přidělí dynamicky při vytvoření disku hello.
* Azure podporuje pouze virtuální počítače generace 1. Virtuální počítač generace 1 můžete převést z formátu souboru virtuálního pevného disku VHDX toohello a dynamicky se zvětšující tooa disk pevné velikosti. Nelze změnit generaci virtuálního počítače. Další informace najdete v tématu [by měl vytvořit virtuální počítač generace 1 nebo 2 v Hyper-V?](https://technet.microsoft.com/windows-server-docs/compute/hyper-v/plan/should-i-create-a-generation-1-or-2-virtual-machine-in-hyper-v).
* Hello maximální povolenou velikost pro hello virtuálního pevného disku je 1,023 GB.
* Při instalaci operačního systému Linux hello, doporučujeme použít standardní oddíly spíše než logické svazku Manager (LVM), což je často hello výchozí pro mnoho instalace. Tento postup zabráníte LVM název je v konfliktu s klonovaný virtuální počítače, zvlášť pokud byste někdy potřebovali tooattach operačního systému disku tooanother identický virtuální počítač pro řešení potíží. [LVM](configure-lvm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) nebo [RAID](configure-raid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) mohou být použity na datové disky.
* Vyžaduje se podpora jádra pro připojení systémy souborů univerzální formát disku (UDF). Při prvním spuštění v Azure, hello formátu UDF médiu, které se předá připojené toohello hosta hello zřizování konfigurace toohello Linux virtuálního počítače. Hello Azure Linux Agent musí být schopný toomount hello UDF soubor systému tooread své konfiguraci a zřízení hello virtuálního počítače.
* Verze hello Linux jádra, které jsou starší než 2.6.37 nepodporují přístup k nerovnoměrné paměti (NUMA) s technologií Hyper-V s větší velikostí virtuálních počítačů. -Li tento problém ovlivňuje především starší distribuce, které používají hello proti proudu Red Hat 2.6.32 jádra která byla opravena v RHEL 6.6 (jádra 2.6.32 504). Systémy, které spouštějí vlastní jádra, které jsou starší než 2.6.37 nebo na základě RHEL jádra, která jsou starší než 2.6.32-504 musíte nastavit hello `numa=off` spouštění na příkazovém řádku jádra hello v grub.conf parametr. Další informace najdete v tématu Red Hat [KB 436883](https://access.redhat.com/solutions/436883).
* Nekonfigurujte přepnutí oddílu na disku operačního systému hello. Hello Linux Agent může být nakonfigurované toocreate odkládací soubor na disku hello dočasných prostředků.  Další informace o této naleznete v hello následující kroky.
* Všechny virtuální pevné disky musí mít velikostí, které jsou násobky 1 MB.

### <a name="prepare-a-rhel-6-virtual-machine-from-hyper-v-manager"></a>Příprava systému RHEL 6 virtuálního počítače ze Správce technologie Hyper-V

1. Ve Správci technologie Hyper-V vyberte hello virtuálního počítače.

2. Klikněte na tlačítko **připojit** tooopen okna konzoly pro virtuální počítač hello.

3. V systému RHEL 6 NetworkManager může narušovat hello Azure Linux agent. Odinstalaci tohoto balíčku spuštěním hello následující příkaz:
   
        # sudo rpm -e --nodeps NetworkManager

4. Vytvořte nebo upravte hello `/etc/sysconfig/network` souboru a přidejte hello následující text:
   
        NETWORKING=yes
        HOSTNAME=localhost.localdomain

5. Vytvořte nebo upravte hello `/etc/sysconfig/network-scripts/ifcfg-eth0` souboru a přidejte hello následující text:
   
        DEVICE=eth0
        ONBOOT=yes
        BOOTPROTO=dhcp
        TYPE=Ethernet
        USERCTL=no
        PEERDNS=yes
        IPV6INIT=no

6. (Nebo odebrat) hello udev pravidla tooavoid generování statická pravidla pro rozhraní sítě Ethernet hello. Tato pravidla způsobit problémy při klonování virtuálního počítače v Microsoft Azure nebo Hyper-V:

        # sudo ln -s /dev/null /etc/udev/rules.d/75-persistent-net-generator.rules
        
        # sudo rm -f /etc/udev/rules.d/70-persistent-net.rules

7. Ujistěte se, že hello síťová služba se spustí při spuštění spuštěním hello následující příkaz:

        # sudo chkconfig network on

8. Zaregistrujte předplatné Red Hat tooenable hello instalace balíčků z úložiště RHEL hello spuštěním hello následující příkaz:

        # sudo subscription-manager register --auto-attach --username=XXX --password=XXX

9. balíček WALinuxAgent Hello `WALinuxAgent-<version>`, bylo posunuto toohello Red Hat funkce úložiště. Povolení funkce úložiště hello spuštěním hello následující příkaz:

        # subscription-manager repos --enable=rhel-6-server-extras-rpms

10. Upravte hello jádra spouštěcí řádku grub konfigurace tooinclude další jádra parametry pro Azure. toodo tato úprava, otevřete `/boot/grub/menu.lst` v textovém editoru a ujistěte se, že jádra výchozí hello zahrnuje hello následující parametry:
    
        console=ttyS0 earlyprintk=ttyS0 rootdelay=300
    
    To také zajistí, že všechny zprávy konzoly jsou odesílány toohello první sériového portu, který může být užitečné Azure podporu pro ladění problémů.
    
    Kromě toho doporučujeme odebrat hello následující parametry:
    
        rhgb quiet crashkernel=auto
    
    Grafické a quiet spouštěcí nejsou užitečné při cloudovém prostředí, kde chceme, aby všechny protokoly toobe hello odeslané toohello sériového portu.  Můžete ponechat hello `crashkernel` možnost nakonfigurované v případě potřeby. Všimněte si, že tento parametr snižuje hello množství dostupné paměti ve virtuálním počítači hello 128 MB a víc. Tato konfigurace může způsobovat problémy menší velikostí virtuálního počítače.

    >[!Important]
    RHEL verze 6.5 a starší, musíte taky nastavit hello `numa=off` jádra parametr. V tématu Red Hat [KB 436883](https://access.redhat.com/solutions/436883).

11. Ujistěte se, tento server hello zabezpečené shell (SSH) je nainstalována a nakonfigurována toostart při spuštění, který je obvykle výchozí hello. Upravte hello tooinclude /etc/ssh/sshd_config následující řádek:

        ClientAliveInterval 180

12. Nainstalujte hello Azure Linux Agent spuštěním hello následující příkaz:

        # sudo yum install WALinuxAgent

        # sudo chkconfig waagent on

    Instalaci balíčku WALinuxAgent hello odebere hello NetworkManager a NetworkManager gnome balíčky, pokud nebyly již odebrány v kroku 3.

13. Nevytvářejte odkládacího prostoru na disku operačního systému hello.

    Hello Azure Linux Agent mohou automaticky konfigurovat odkládacího souboru pomocí disku hello místní prostředek, který je připojený toohello virtuální počítač po zřízení hello virtuálního počítače na platformě Azure. Všimněte si, že hello místní disk prostředků je dočasný a že ji může být vyprázdněna při hello virtuálního počítače je zrušit. Po instalaci hello Azure Linux Agent hello předchozího kroku, upravte hello následující parametry v /etc/waagent.conf odpovídajícím způsobem:

        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this toowhatever you need it toobe.

14. Zrušení registrace předplatného hello (v případě potřeby) tak, že spustíte následující příkaz hello:

        # sudo subscription-manager unregister

15. Spusťte hello následující příkazy toodeprovision hello virtuálního počítače a jeho přípravu pro zřizování na Azure:

        # sudo waagent -force -deprovision

        # export HISTSIZE=0

        # logout

16. Klikněte na tlačítko **akce** > **vypnout** ve Správci technologie Hyper-V. Svůj disk VHD Linux je nyní připraven toobe nahrán tooAzure.


### <a name="prepare-a-rhel-7-virtual-machine-from-hyper-v-manager"></a>Příprava RHEL 7 virtuálního počítače ze Správce technologie Hyper-V

1. Ve Správci technologie Hyper-V vyberte hello virtuálního počítače.

2. Klikněte na tlačítko **připojit** tooopen okna konzoly pro virtuální počítač hello.

3. Vytvořte nebo upravte hello `/etc/sysconfig/network` souboru a přidejte hello následující text:
   
        NETWORKING=yes
        HOSTNAME=localhost.localdomain

4. Vytvořte nebo upravte hello `/etc/sysconfig/network-scripts/ifcfg-eth0` souboru a přidejte hello následující text:
   
        DEVICE=eth0
        ONBOOT=yes
        BOOTPROTO=dhcp
        TYPE=Ethernet
        USERCTL=no
        PEERDNS=yes
        IPV6INIT=no
        NM_CONTROLLED=no

5. Ujistěte se, že hello síťová služba se spustí při spuštění spuštěním hello následující příkaz:

        # sudo chkconfig network on

6. Zaregistrujte předplatné Red Hat tooenable hello instalace balíčků z úložiště RHEL hello spuštěním hello následující příkaz:

        # sudo subscription-manager register --auto-attach --username=XXX --password=XXX

7. Upravte hello jádra spouštěcí řádku grub konfigurace tooinclude další jádra parametry pro Azure. toodo tato úprava, otevřete `/etc/default/grub` v textovém editoru a upravit hello `GRUB_CMDLINE_LINUX` parametr. Například:
   
        GRUB_CMDLINE_LINUX="rootdelay=300 console=ttyS0 earlyprintk=ttyS0 net.ifnames=0"
   
   To také zajistí, že všechny zprávy konzoly jsou odesílány toohello první sériového portu, který může být užitečné Azure podporu pro ladění problémů. Tato konfigurace taky vypne hello nové RHEL 7 zásady vytváření názvů pro síťové adaptéry. Kromě toho doporučujeme odebrat hello následující parametry:
   
        rhgb quiet crashkernel=auto
   
    Grafické a quiet spouštěcí nejsou užitečné při cloudovém prostředí, kde chceme, aby všechny protokoly toobe hello odeslané toohello sériového portu. Můžete ponechat hello `crashkernel` možnost nakonfigurované v případě potřeby. Všimněte si, že tento parametr snižuje hello množství dostupné paměti v hello virtuálního počítače pomocí 128 MB nebo víc, což může způsobovat problémy menší velikostí virtuálního počítače.

8. Po dokončení úprav `/etc/default/grub`spusťte hello následující příkaz toorebuild hello grub konfigurace:

        # sudo grub2-mkconfig -o /boot/grub2/grub.cfg

9. Ujistěte se, že hello SSH server je nainstalován a nakonfigurován toostart při spuštění, který je obvykle výchozí hello. Upravit `/etc/ssh/sshd_config` tooinclude hello následující řádek:

        ClientAliveInterval 180

10. balíček WALinuxAgent Hello `WALinuxAgent-<version>`, bylo posunuto toohello Red Hat funkce úložiště. Povolení funkce úložiště hello spuštěním hello následující příkaz:

        # subscription-manager repos --enable=rhel-7-server-extras-rpms

11. Nainstalujte hello Azure Linux Agent spuštěním hello následující příkaz:

        # sudo yum install WALinuxAgent

        # sudo systemctl enable waagent.service

12. Nevytvářejte odkládacího prostoru na disku operačního systému hello.

    Hello Azure Linux Agent mohou automaticky konfigurovat odkládacího souboru pomocí disku hello místní prostředek, který je připojený toohello virtuální počítač po zřízení hello virtuálního počítače na platformě Azure. Upozorňujeme, že hello místní prostředek disk je dočasný a může být vyprázdnit po deprovisioned hello virtuálního počítače. Po instalaci hello Azure Linux Agent v předchozím kroku hello se upravit hello následující parametry v `/etc/waagent.conf` odpovídajícím způsobem:

        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this toowhatever you need it toobe.

13. Pokud chcete toounregister hello předplatné, spusťte následující příkaz hello:

        # sudo subscription-manager unregister

14. Spusťte hello následující příkazy toodeprovision hello virtuálního počítače a jeho přípravu pro zřizování na Azure:

        # sudo waagent -force -deprovision

        # export HISTSIZE=0

        # logout

15. Klikněte na tlačítko **akce** > **vypnout** ve Správci technologie Hyper-V. Svůj disk VHD Linux je nyní připraven toobe nahrán tooAzure.


## <a name="prepare-a-red-hat-based-virtual-machine-from-kvm"></a>Příprava virtuálního počítače, na základě Red Hat z KVM
### <a name="prepare-a-rhel-6-virtual-machine-from-kvm"></a>Příprava systému RHEL 6 virtuálního počítače z KVM

1. Stáhněte z webu Red Hat hello hello KVM bitovou kopii systému RHEL 6.

2. Nastavení kořenové heslo.

    Generovat zašifrované heslo a zkopírujte hello výstup hello příkazu:

        # openssl passwd -1 changeme

    Nastavení kořenové heslo s guestfish:
        
        # guestfish --rw -a <image-name>
        > <fs> run
        > <fs> list-filesystems
        > <fs> mount /dev/sda1 /
        > <fs> vi /etc/shadow
        > <fs> exit

   Změna hello druhé pole hello kořenového uživatele z "!" toohello zašifrované heslo.

3. Vytvoření virtuálního počítače v KVM z hello qcow2 image. Nastavte typ disku hello příliš**qcow2**a nastavte model zařízení rozhraní virtuální sítě hello příliš**virtio**. Potom spusťte hello virtuálního počítače a přihlaste se jako kořenový.

4. Vytvořte nebo upravte hello `/etc/sysconfig/network` souboru a přidejte hello následující text:
   
        NETWORKING=yes
        HOSTNAME=localhost.localdomain

5. Vytvořte nebo upravte hello `/etc/sysconfig/network-scripts/ifcfg-eth0` souboru a přidejte hello následující text:
   
        DEVICE=eth0
        ONBOOT=yes
        BOOTPROTO=dhcp
        TYPE=Ethernet
        USERCTL=no
        PEERDNS=yes
        IPV6INIT=no

6. (Nebo odebrat) hello udev pravidla tooavoid generování statická pravidla pro rozhraní sítě Ethernet hello. Tato pravidla způsobit problémy při klonování virtuálního počítače v Azure nebo Hyper-V:

        # sudo ln -s /dev/null /etc/udev/rules.d/75-persistent-net-generator.rules

        # sudo rm -f /etc/udev/rules.d/70-persistent-net.rules

7. Ujistěte se, že hello síťová služba se spustí při spuštění spuštěním hello následující příkaz:

        # chkconfig network on

8. Zaregistrujte předplatné Red Hat tooenable hello instalace balíčků z úložiště RHEL hello spuštěním hello následující příkaz:

        # subscription-manager register --auto-attach --username=XXX --password=XXX

9. Upravte hello jádra spouštěcí řádku grub konfigurace tooinclude další jádra parametry pro Azure. toodo tuto konfiguraci, otevřete `/boot/grub/menu.lst` v textovém editoru a ujistěte se, že jádra výchozí hello zahrnuje hello následující parametry:
    
        console=ttyS0 earlyprintk=ttyS0 rootdelay=300
    
    To také zajistí, že všechny zprávy konzoly jsou odesílány toohello první sériového portu, který může být užitečné Azure podporu pro ladění problémů.
    
    Kromě toho doporučujeme odebrat hello následující parametry:
    
        rhgb quiet crashkernel=auto
    
    Grafické a quiet spouštěcí nejsou užitečné při cloudovém prostředí, kde chceme, aby všechny protokoly toobe hello odeslané toohello sériového portu. Můžete ponechat hello `crashkernel` možnost nakonfigurované v případě potřeby. Všimněte si, že tento parametr snižuje hello množství dostupné paměti v hello virtuálního počítače pomocí 128 MB nebo víc, což může způsobovat problémy menší velikostí virtuálního počítače.

    >[!Important]
    RHEL verze 6.5 a starší, musíte taky nastavit hello `numa=off` jádra parametr. V tématu Red Hat [KB 436883](https://access.redhat.com/solutions/436883).

10. Přidejte tooinitramfs modulů technologie Hyper-V:  

    Upravit `/etc/dracut.conf`a přidejte hello následující obsah:

        add_drivers+="hv_vmbus hv_netvsc hv_storvsc"

    Znovu sestavte initramfs:

        # dracut -f -v

11. Odinstalace init cloudu:

        # yum remove cloud-init

12. Ujistěte se, že server SSH hello je nainstalován a nakonfigurován toostart při spuštění:

        # chkconfig sshd on

    Upravte hello tooinclude /etc/ssh/sshd_config následující řádky:

        PasswordAuthentication yes
        ClientAliveInterval 180

13. balíček WALinuxAgent Hello `WALinuxAgent-<version>`, bylo posunuto toohello Red Hat funkce úložiště. Povolení funkce úložiště hello spuštěním hello následující příkaz:

        # subscription-manager repos --enable=rhel-6-server-extras-rpms

14. Nainstalujte hello Azure Linux Agent spuštěním hello následující příkaz:

        # yum install WALinuxAgent

        # chkconfig waagent on

15. Hello Azure Linux Agent mohou automaticky konfigurovat odkládacího souboru pomocí disku hello místní prostředek, který je připojený toohello virtuální počítač po zřízení hello virtuálního počítače na platformě Azure. Upozorňujeme, že hello místní prostředek disk je dočasný a může být vyprázdnit po deprovisioned hello virtuálního počítače. Po instalaci hello Azure Linux Agent v předchozím kroku hello se upravit hello následující parametry v **/etc/waagent.conf** odpovídajícím způsobem:

        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this toowhatever you need it toobe.

16. Zrušení registrace předplatného hello (v případě potřeby) tak, že spustíte následující příkaz hello:

        # subscription-manager unregister

17. Spusťte hello následující příkazy toodeprovision hello virtuálního počítače a jeho přípravu pro zřizování na Azure:

        # waagent -force -deprovision

        # export HISTSIZE=0

        # logout

18. Vypnete virtuální počítač hello v KVM.

19. Převod formátu virtuálního pevného disku toohello image qcow2 hello.

    Nejprve převeďte formát tooraw hello obrázku:

        # qemu-img convert -f qcow2 -O raw rhel-6.8.qcow2 rhel-6.8.raw

    Ujistěte se, že velikost hello hello nezpracovaná bitové kopie je v souladu s 1 MB. Jinak zaokrouhlí nahoru hello velikost tooalign s 1 MB:

        # MB=$((1024*1024))
        # size=$(qemu-img info -f raw --output json "rhel-6.8.raw" | \
          gawk 'match($0, /"virtual-size": ([0-9]+),/, val) {print val[1]}')

        # rounded_size=$((($size/$MB + 1)*$MB))
        # qemu-img resize rhel-6.8.raw $rounded_size

    Převést tooa hello nezpracovaná disku pevné velikosti virtuálního pevného disku:

        # qemu-img convert -f raw -o subformat=fixed -O vpc rhel-6.8.raw rhel-6.8.vhd


### <a name="prepare-a-rhel-7-virtual-machine-from-kvm"></a>Příprava virtuálního počítače, RHEL 7 z KVM

1. Obrázek KVM hello RHEL 7 stáhněte z webu Red Hat hello. Tento postup používá jako příklad hello RHEL 7.

2. Nastavení kořenové heslo.

    Generovat zašifrované heslo a zkopírujte hello výstup hello příkazu:

        # openssl passwd -1 changeme

    Nastavení kořenové heslo s guestfish:

        # guestfish --rw -a <image-name>
        > <fs> run
        > <fs> list-filesystems
        > <fs> mount /dev/sda1 /
        > <fs> vi /etc/shadow
        > <fs> exit

   Změna hello druhé pole kořenové uživatele z "!" toohello zašifrované heslo.

3. Vytvoření virtuálního počítače v KVM z hello qcow2 image. Nastavte typ disku hello příliš**qcow2**a nastavte model zařízení rozhraní virtuální sítě hello příliš**virtio**. Potom spusťte hello virtuálního počítače a přihlaste se jako kořenový.

4. Vytvořte nebo upravte hello `/etc/sysconfig/network` souboru a přidejte hello následující text:
   
        NETWORKING=yes
        HOSTNAME=localhost.localdomain

5. Vytvořte nebo upravte hello `/etc/sysconfig/network-scripts/ifcfg-eth0` souboru a přidejte hello následující text:
   
        DEVICE=eth0
        ONBOOT=yes
        BOOTPROTO=dhcp
        TYPE=Ethernet
        USERCTL=no
        PEERDNS=yes
        IPV6INIT=no
        NM_CONTROLLED=no

6. Ujistěte se, že hello síťová služba se spustí při spuštění spuštěním hello následující příkaz:

        # chkconfig network on

7. Zaregistrujte předplatné Red Hat tooenable instalace balíčků z úložiště RHEL hello spuštěním hello následující příkaz:

        # subscription-manager register --auto-attach --username=XXX --password=XXX

8. Upravte hello jádra spouštěcí řádku grub konfigurace tooinclude další jádra parametry pro Azure. toodo tuto konfiguraci, otevřete `/etc/default/grub` v textovém editoru a upravit hello `GRUB_CMDLINE_LINUX` parametr. Například:
   
        GRUB_CMDLINE_LINUX="rootdelay=300 console=ttyS0 earlyprintk=ttyS0 net.ifnames=0"
   
   Tento příkaz také zajistí, že všechny zprávy konzoly jsou odesílány toohello první sériového portu, který může být užitečné Azure podporu pro ladění problémů. příkaz Hello vypne také hello nové RHEL 7 zásady vytváření názvů pro síťové adaptéry. Kromě toho doporučujeme odebrat hello následující parametry:
   
        rhgb quiet crashkernel=auto
   
    Grafické a quiet spouštěcí nejsou užitečné při cloudovém prostředí, kde chceme, aby všechny protokoly toobe hello odeslané toohello sériového portu. Můžete ponechat hello `crashkernel` možnost nakonfigurované v případě potřeby. Všimněte si, že tento parametr snižuje hello množství dostupné paměti v hello virtuálního počítače pomocí 128 MB nebo víc, což může způsobovat problémy menší velikostí virtuálního počítače.

9. Po dokončení úprav `/etc/default/grub`spusťte hello následující příkaz toorebuild hello grub konfigurace:

        # grub2-mkconfig -o /boot/grub2/grub.cfg

10. Přidejte do initramfs moduly Hyper-V.

    Upravit `/etc/dracut.conf` a přidejte obsah:

        add_drivers+="hv_vmbus hv_netvsc hv_storvsc"

    Znovu sestavte initramfs:

        # dracut -f -v

11. Odinstalace init cloudu:

        # yum remove cloud-init

12. Ujistěte se, že server SSH hello je nainstalován a nakonfigurován toostart při spuštění:

        # systemctl enable sshd

    Upravte hello tooinclude /etc/ssh/sshd_config následující řádky:

        PasswordAuthentication yes
        ClientAliveInterval 180

13. balíček WALinuxAgent Hello `WALinuxAgent-<version>`, bylo posunuto toohello Red Hat funkce úložiště. Povolení funkce úložiště hello spuštěním hello následující příkaz:

        # subscription-manager repos --enable=rhel-7-server-extras-rpms

14. Nainstalujte hello Azure Linux Agent spuštěním hello následující příkaz:

        # yum install WALinuxAgent

    Povolení služby příkaz waagent hello:

        # systemctl enable waagent.service

15. Nevytvářejte odkládacího prostoru na disku operačního systému hello.

    Hello Azure Linux Agent mohou automaticky konfigurovat odkládacího souboru pomocí disku hello místní prostředek, který je připojený toohello virtuální počítač po zřízení hello virtuálního počítače na platformě Azure. Upozorňujeme, že hello místní prostředek disk je dočasný a může být vyprázdnit po deprovisioned hello virtuálního počítače. Po instalaci hello Azure Linux Agent v předchozím kroku hello se upravit hello následující parametry v `/etc/waagent.conf` odpovídajícím způsobem:

        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this toowhatever you need it toobe.

16. Zrušení registrace předplatného hello (v případě potřeby) tak, že spustíte následující příkaz hello:

        # subscription-manager unregister

17. Spusťte hello následující příkazy toodeprovision hello virtuálního počítače a jeho přípravu pro zřizování na Azure:

        # sudo waagent -force -deprovision

        # export HISTSIZE=0

        # logout

18. Vypnete virtuální počítač hello v KVM.

19. Převod formátu virtuálního pevného disku toohello image qcow2 hello.

    Nejprve převeďte formát tooraw hello obrázku:

        # qemu-img convert -f qcow2 -O raw rhel-7.3.qcow2 rhel-7.3.raw

    Ujistěte se, že velikost hello hello nezpracovaná bitové kopie je v souladu s 1 MB. Jinak zaokrouhlí nahoru hello velikost tooalign s 1 MB:

        # MB=$((1024*1024))
        # size=$(qemu-img info -f raw --output json "rhel-6.8.raw" | \
          gawk 'match($0, /"virtual-size": ([0-9]+),/, val) {print val[1]}')

        # rounded_size=$((($size/$MB + 1)*$MB))
        # qemu-img resize rhel-6.8.raw $rounded_size

    Převést tooa hello nezpracovaná disku pevné velikosti virtuálního pevného disku:

        # qemu-img convert -f raw -o subformat=fixed -O vpc rhel-7.3.raw rhel-7.3.vhd

## <a name="prepare-a-red-hat-based-virtual-machine-from-vmware"></a>Příprava virtuálního počítače, na základě Red Hat z VMware
### <a name="prerequisites"></a>Požadavky
V této části se předpokládá, že jste již nainstalovali RHEL virtuálního počítače v prostředí VMware. Podrobnosti o tom, jak tooinstall operačního systému v prostředí VMware, najdete v části [Průvodce instalací operačního systému hosta VMware](http://partnerweb.vmware.com/GOSIG/home.html).

* Při instalaci operačního systému Linux hello, doporučujeme použít standardní oddíly spíše než LVM, což je často hello výchozí pro mnoho instalace. Tím se vyhnete LVM název je v konfliktu s naklonovaný virtuální počítač, zvlášť pokud disk operačního systému pro řešení potíží s někdy potřebuje toobe připojené tooanother virtuálního počítače. LVM nebo RAID lze použít v datových disků Pokud dáváte přednost.
* Nekonfigurujte přepnutí oddílu na disku operačního systému hello. Můžete nakonfigurovat hello Linux agent toocreate odkládací soubor na disku hello dočasných prostředků. Další informace o tom najdete v hello kroky, které následují.
* Když vytvoříte hello virtuální pevný disk, vyberte **virtuálního disku úložiště jako samostatný soubor**.

### <a name="prepare-a-rhel-6-virtual-machine-from-vmware"></a>Příprava systému RHEL 6 virtuálního počítače z VMware
1. V systému RHEL 6 NetworkManager může narušovat hello Azure Linux agent. Odinstalaci tohoto balíčku spuštěním hello následující příkaz:
   
        # sudo rpm -e --nodeps NetworkManager

2. Vytvořte soubor s názvem **sítě** v hello etc/sysconfig/adresář, který obsahuje hello následující text:

        NETWORKING=yes
        HOSTNAME=localhost.localdomain

3. Vytvořte nebo upravte hello `/etc/sysconfig/network-scripts/ifcfg-eth0` souboru a přidejte hello následující text:
   
        DEVICE=eth0
        ONBOOT=yes
        BOOTPROTO=dhcp
        TYPE=Ethernet
        USERCTL=no
        PEERDNS=yes
        IPV6INIT=no

4. (Nebo odebrat) hello udev pravidla tooavoid generování statická pravidla pro rozhraní sítě Ethernet hello. Tato pravidla způsobit problémy při klonování virtuálního počítače v Azure nebo Hyper-V:

        # sudo ln -s /dev/null /etc/udev/rules.d/75-persistent-net-generator.rules

        # sudo rm -f /etc/udev/rules.d/70-persistent-net.rules

5. Ujistěte se, že hello síťová služba se spustí při spuštění spuštěním hello následující příkaz:

        # sudo chkconfig network on

6. Zaregistrujte předplatné Red Hat tooenable hello instalace balíčků z úložiště RHEL hello spuštěním hello následující příkaz:

        # sudo subscription-manager register --auto-attach --username=XXX --password=XXX

7. balíček WALinuxAgent Hello `WALinuxAgent-<version>`, bylo posunuto toohello Red Hat funkce úložiště. Povolení funkce úložiště hello spuštěním hello následující příkaz:

        # subscription-manager repos --enable=rhel-6-server-extras-rpms

8. Upravte hello jádra spouštěcí řádku grub konfigurace tooinclude další jádra parametry pro Azure. toodo tuto, otevřete `/etc/default/grub` v textovém editoru a upravit hello `GRUB_CMDLINE_LINUX` parametr. Například:
   
        GRUB_CMDLINE_LINUX="rootdelay=300 console=ttyS0 earlyprintk=ttyS0"
   
   To také zajistí, že všechny zprávy konzoly jsou odesílány toohello první sériového portu, který může být užitečné Azure podporu pro ladění problémů. Kromě toho doporučujeme odebrat hello následující parametry:
   
        rhgb quiet crashkernel=auto
   
    Grafické a quiet spouštěcí nejsou užitečné při cloudovém prostředí, kde chceme, aby všechny protokoly toobe hello odeslané toohello sériového portu. Můžete ponechat hello `crashkernel` možnost nakonfigurované v případě potřeby. Všimněte si, že tento parametr snižuje hello množství dostupné paměti v hello virtuálního počítače pomocí 128 MB nebo víc, což může způsobovat problémy menší velikostí virtuálního počítače.

9. Přidejte tooinitramfs modulů technologie Hyper-V:

    Upravit `/etc/dracut.conf`a přidejte hello následující obsah:

        add_drivers+="hv_vmbus hv_netvsc hv_storvsc"

    Znovu sestavte initramfs:

        # dracut -f -v

10. Ujistěte se, že hello SSH server je nainstalován a nakonfigurován toostart při spuštění, který je obvykle výchozí hello. Upravit `/etc/ssh/sshd_config` tooinclude hello následující řádek:

    ClientAliveInterval 180

11. Nainstalujte hello Azure Linux Agent spuštěním hello následující příkaz:

        # sudo yum install WALinuxAgent

        # sudo chkconfig waagent on

12. Nevytvářejte odkládacího prostoru na disku operačního systému hello.

    Hello Azure Linux Agent mohou automaticky konfigurovat odkládacího souboru pomocí disku hello místní prostředek, který je připojený toohello virtuální počítač po zřízení hello virtuálního počítače na platformě Azure. Upozorňujeme, že hello místní prostředek disk je dočasný a může být vyprázdnit po deprovisioned hello virtuálního počítače. Po instalaci hello Azure Linux Agent v předchozím kroku hello se upravit hello následující parametry v `/etc/waagent.conf` odpovídajícím způsobem:

        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this toowhatever you need it toobe.

13. Zrušení registrace předplatného hello (v případě potřeby) tak, že spustíte následující příkaz hello:

        # sudo subscription-manager unregister

14. Spusťte hello následující příkazy toodeprovision hello virtuálního počítače a jeho přípravu pro zřizování na Azure:

        # sudo waagent -force -deprovision

        # export HISTSIZE=0

        # logout

15. Vypnout hello virtuální počítač a proveďte převod souboru VHD tooa soubor VMDK hello.

    Nejprve převeďte formát tooraw hello obrázku:

        # qemu-img convert -f vmdk -O raw rhel-6.8.vmdk rhel-6.8.raw

    Ujistěte se, že velikost hello hello nezpracovaná bitové kopie je v souladu s 1 MB. Jinak zaokrouhlí nahoru hello velikost tooalign s 1 MB:

        # MB=$((1024*1024))
        # size=$(qemu-img info -f raw --output json "rhel-6.8.raw" | \
          gawk 'match($0, /"virtual-size": ([0-9]+),/, val) {print val[1]}')

        # rounded_size=$((($size/$MB + 1)*$MB))
        # qemu-img resize rhel-6.8.raw $rounded_size

    Převést tooa hello nezpracovaná disku pevné velikosti virtuálního pevného disku:

        # qemu-img convert -f raw -o subformat=fixed -O vpc rhel-6.8.raw rhel-6.8.vhd

### <a name="prepare-a-rhel-7-virtual-machine-from-vmware"></a>Příprava virtuálního počítače, RHEL 7 z VMware
1. Vytvořte nebo upravte hello `/etc/sysconfig/network` souboru a přidejte hello následující text:
   
        NETWORKING=yes
        HOSTNAME=localhost.localdomain

2. Vytvořte nebo upravte hello `/etc/sysconfig/network-scripts/ifcfg-eth0` souboru a přidejte hello následující text:
   
        DEVICE=eth0
        ONBOOT=yes
        BOOTPROTO=dhcp
        TYPE=Ethernet
        USERCTL=no
        PEERDNS=yes
        IPV6INIT=no
        NM_CONTROLLED=no

3. Ujistěte se, že hello síťová služba se spustí při spuštění spuštěním hello následující příkaz:

        # sudo chkconfig network on

4. Zaregistrujte předplatné Red Hat tooenable hello instalace balíčků z úložiště RHEL hello spuštěním hello následující příkaz:

        # sudo subscription-manager register --auto-attach --username=XXX --password=XXX

5. Upravte hello jádra spouštěcí řádku grub konfigurace tooinclude další jádra parametry pro Azure. toodo tato úprava, otevřete `/etc/default/grub` v textovém editoru a upravit hello `GRUB_CMDLINE_LINUX` parametr. Například:
   
        GRUB_CMDLINE_LINUX="rootdelay=300 console=ttyS0 earlyprintk=ttyS0 net.ifnames=0"
   
   Tato konfigurace taky zajišťuje, že všechny zprávy konzoly jsou odesílány toohello první sériového portu, který může být užitečné Azure podporu pro ladění problémů. Je také vypne hello nové RHEL 7 zásady vytváření názvů pro síťové adaptéry. Kromě toho doporučujeme odebrat hello následující parametry:
   
        rhgb quiet crashkernel=auto
   
    Grafické a quiet spouštěcí nejsou užitečné při cloudovém prostředí, kde chceme, aby všechny protokoly toobe hello odeslané toohello sériového portu. Můžete ponechat hello `crashkernel` možnost nakonfigurované v případě potřeby. Všimněte si, že tento parametr snižuje hello množství dostupné paměti v hello virtuálního počítače pomocí 128 MB nebo víc, což může způsobovat problémy menší velikostí virtuálního počítače.

6. Po dokončení úprav `/etc/default/grub`spusťte hello následující příkaz toorebuild hello grub konfigurace:

        # sudo grub2-mkconfig -o /boot/grub2/grub.cfg

7. Přidejte tooinitramfs modulů technologie Hyper-V.

    Upravit `/etc/dracut.conf`, přidejte obsah:

        add_drivers+="hv_vmbus hv_netvsc hv_storvsc"

    Znovu sestavte initramfs:

        # dracut -f -v

8. Ujistěte se, že server SSH hello je nainstalován a nakonfigurován toostart při spuštění. Toto nastavení je obvykle výchozí hello. Upravit `/etc/ssh/sshd_config` tooinclude hello následující řádek:

        ClientAliveInterval 180

9. balíček WALinuxAgent Hello `WALinuxAgent-<version>`, bylo posunuto toohello Red Hat funkce úložiště. Povolení funkce úložiště hello spuštěním hello následující příkaz:

        # subscription-manager repos --enable=rhel-7-server-extras-rpms

10. Nainstalujte hello Azure Linux Agent spuštěním hello následující příkaz:

        # sudo yum install WALinuxAgent

        # sudo systemctl enable waagent.service

11. Nevytvářejte odkládacího prostoru na disku operačního systému hello.

    Hello Azure Linux Agent mohou automaticky konfigurovat odkládacího souboru pomocí disku hello místní prostředek, který je připojený toohello virtuální počítač po zřízení hello virtuálního počítače na platformě Azure. Upozorňujeme, že hello místní prostředek disk je dočasný a může být vyprázdnit po deprovisioned hello virtuálního počítače. Po instalaci hello Azure Linux Agent v předchozím kroku hello se upravit hello následující parametry v `/etc/waagent.conf` odpovídajícím způsobem:

        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this toowhatever you need it toobe.

12. Pokud chcete toounregister hello předplatné, spusťte následující příkaz hello:

        # sudo subscription-manager unregister

13. Spusťte hello následující příkazy toodeprovision hello virtuálního počítače a jeho přípravu pro zřizování na Azure:

        # sudo waagent -force -deprovision

        # export HISTSIZE=0

        # logout

14. Vypnout hello virtuální počítač a proveďte převod formátu VHD toohello soubor VMDK hello.

    Nejprve převeďte formát tooraw hello obrázku:

        # qemu-img convert -f vmdk -O raw rhel-7.3.vmdk rhel-7.3.raw

    Ujistěte se, že velikost hello hello nezpracovaná bitové kopie je v souladu s 1 MB. Jinak zaokrouhlí nahoru hello velikost tooalign s 1 MB:

        # MB=$((1024*1024))
        # size=$(qemu-img info -f raw --output json "rhel-6.8.raw" | \
          gawk 'match($0, /"virtual-size": ([0-9]+),/, val) {print val[1]}')

        # rounded_size=$((($size/$MB + 1)*$MB))
        # qemu-img resize rhel-6.8.raw $rounded_size

    Převést tooa hello nezpracovaná disku pevné velikosti virtuálního pevného disku:

        # qemu-img convert -f raw -o subformat=fixed -O vpc rhel-7.3.raw rhel-7.3.vhd

## <a name="prepare-a-red-hat-based-virtual-machine-from-an-iso-by-using-a-kickstart-file-automatically"></a>Příprava virtuálního počítače, na základě Red Hat ze souboru ISO pomocí souboru kickstart automaticky
### <a name="prepare-a-rhel-7-virtual-machine-from-a-kickstart-file"></a>Příprava virtuálního počítače ze souboru kickstart RHEL 7

1.  Vytvořte soubor kickstart, který obsahuje následující obsah hello a uložte soubor hello. Podrobnosti o kickstart instalace najdete v tématu hello [Průvodce instalací Kickstart](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/7/html/Installation_Guide/chap-kickstart-installations.html).

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

2. Umístěte soubor kickstart hello kde hello instalace systému k němu přístup.

3. Ve Správci technologie Hyper-V vytvořte nový virtuální počítač. Na hello **připojit virtuální pevný Disk** vyberte **připojit virtuální pevný disk později**a hello dokončení Průvodce novým virtuálním počítačem.

4. Otevřete nastavení virtuálního počítače hello:

    a.  Připojte nový virtuální počítač toohello virtuální pevný disk. Ujistěte se, že tooselect **formátu virtuálního pevného disku** a **pevné velikosti**.

    b.  Připojte hello instalace jednotku DVD toohello ISO.

    c.  Nastavení systému BIOS tooboot hello z disku CD-ROM.

5. Spusťte virtuální počítač hello. Jakmile se zobrazí Průvodce instalací hello, stiskněte klávesu **kartě** možnosti spuštění tooconfigure hello.

6. Zadejte `inst.ks=<hello location of hello kickstart file>` na konci hello hello možnosti spuštění a stiskněte klávesu **Enter**.

7. Počkejte toofinish instalace hello. Po dokončení, hello virtuální počítač se vypne automaticky. Svůj disk VHD Linux je nyní připraven toobe nahrán tooAzure.

## <a name="known-issues"></a>Známé problémy
### <a name="hello-hyper-v-driver-could-not-be-included-in-hello-initial-ram-disk-when-using-a-non-hyper-v-hypervisor"></a>ovladač Hello technologie Hyper-V není v počáteční disku RAM hello při použití hypervisor bez technologie Hyper-V

V některých případech instalačních programů Linux nemusí zahrnovat hello ovladače pro Hyper-V v hello počáteční paměť RAM disku (initrd nebo initramfs) Pokud Linux zjistí, zda je spuštěna v prostředí Hyper-V.

Pokud používáte tooprepare systému (tj. Virtualbox, Xen atd.) jiným virtualizačním bitové kopie systému Linux, možná budete muset tooensure initrd toorebuild, který alespoň hello hv_vmbus a hv_storvsc jádra moduly jsou k dispozici na disku počáteční paměť RAM hello. V systémech, které jsou založeny na nadřazený distribuční Red Hat hello alespoň jde o známý problém.

tooresolve tento problém, přidejte tooinitramfs modulů technologie Hyper-V a znovu ji vytvořit:

Upravit `/etc/dracut.conf`a přidejte hello následující obsah:

        add_drivers+="hv_vmbus hv_netvsc hv_storvsc"

Znovu sestavte initramfs:

        # dracut -f -v

Další podrobnosti najdete v tématu hello informace [znovu sestavit initramfs](https://access.redhat.com/solutions/1958).

## <a name="next-steps"></a>Další kroky
Můžete se teď připravena toouse Red Hat Enterprise Linux virtuální pevný disk toocreate nové virtuální počítače v Azure. Pokud je to hello poprvé, že jste odesílání tooAzure soubor VHD hello, najdete v části kroky 2 a 3 v [vytváření a odesílání virtuální pevný disk, který obsahuje operační systém Linux hello](classic/create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).

Další podrobnosti o hello hypervisorů, které jsou certifikované toorun Red Hat Enterprise Linux, najdete v části [hello Red Hat webu](https://access.redhat.com/certified-hypervisors).
