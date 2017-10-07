---
title: "aaaCreate a nahrání virtuálního pevného disku Linux v Azure"
description: "Další informace toocreate a nahrajte Azure virtuálního pevného disku (VHD) obsahující operační systém Linux."
services: virtual-machines-linux
documentationcenter: 
author: szarkos
manager: timlt
editor: tysonn
tags: azure-resource-manager,azure-service-management
ms.assetid: d351396c-95a0-4092-b7bf-c6aae0bbd112
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 02/02/2017
ms.author: szark
ms.openlocfilehash: 208e15c035edb5520fc29ba8e4e71c42b041864d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="information-for-non-endorsed-distributions"></a>Informace pro neschválené distribuce
[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

Hello platformy Azure SLA se vztahuje toovirtual počítačů spuštěných hello operační systém Linux jenom v případě, že je jeden z hello [distribuce schválené](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) se používá. Distribuce s požadovanou konfigurací hello schválené pro všechny distribuce systému Linux, které jsou k dispozici v hello Azure jsou Galerie obrázků.

* [Distribuce schválené pro Linux ve službě Azure-](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Podpora pro Linux obrázků v Microsoft Azure](https://support.microsoft.com/kb/2941892)

Všechny distribuce spuštěné v Azure, bude nutné toomeet počet požadavků toohave prvního tooproperly, spusťte na platformě hello.  Tento článek je komplexní rozhodně není, protože každý distribuční se liší. a je dost možné, že i v případě, že splňujete všechny hello následujících kritérií je stále nutné upravit toosignificantly vaše tooensure systému Linux, který v něm běží správně na platformě hello.

Je z tohoto důvodu, který doporučujeme spouštět některý z našich [Linux na distribuce schválené pro Azure](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) Pokud je to možné. Hello následující články vás provede jak tooprepare hello různé schválené distribucí Linux, které jsou podporovány v Azure:

* **[Na základě centOS distribuce](create-upload-centos.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**
* **[Debian Linux](debian-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**
* **[Oracle Linux](oracle-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**
* **[Red Hat Enterprise Linux](redhat-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**
* **[SLES & openSUSE](suse-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**
* **[Ubuntu](create-upload-ubuntu.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**

Hello zbývající části tohoto článku se soustředí na obecných pokynů pro spuštění distribuční Linux v Azure.

## <a name="general-linux-installation-notes"></a>Poznámky k instalaci obecné Linux
* Formát VHDX Hello není podporován v Azure, pouze **pevný virtuální pevný disk**.  Můžete převést formát tooVHD hello disku pomocí Správce technologie Hyper-V nebo hello rutiny convert-VHD prostředí. Pokud používáte VirtualBox to znamená výběr **pevnou velikost** jako byl proti toohello výchozí přidělí dynamicky při vytváření disku hello.
* Azure podporuje pouze virtuální počítače generace 1. Můžete převést generace 1 virtuálního počítače z formátu souboru virtuálního pevného disku VHDX toohello a dynamicky se zvětšující tooa pevné velikosti disku. Ale nemůžete změnit generaci virtuálního počítače. Další informace najdete v tématu [by měl vytvořit virtuální počítač generace 1 nebo 2 v Hyper-V?](https://technet.microsoft.com/en-us/windows-server-docs/compute/hyper-v/plan/should-i-create-a-generation-1-or-2-virtual-machine-in-hyper-v)
* virtuální pevný disk je 1,023 GB Hello maximální velikost povolená pro hello.
* Při instalaci systému Linux hello je *doporučená* použít standardní oddíly spíše než LVM (často hello výchozí pro mnoho instalace). Tím se vyhnete LVM název je v konfliktu s klonovaný virtuální počítače, zvlášť pokud disk s operačním systémem někdy potřebuje toobe připojené tooanother identické virtuálního počítače pro řešení potíží. [LVM](configure-lvm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) nebo [RAID](configure-raid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) mohou být použity na datové disky.
* Vyžaduje se podpora jádra pro připojení systémy souborů UDF. Při prvním spuštění v Azure hello je předána zřizování konfigurace toohello virtuálního počítače s Linuxem pomocí formátu UDF média, který je připojený toohello hosta. Hello Azure Linux agent musí být schopný toomount hello UDF soubor systému tooread své konfiguraci a zřídit hello virtuálních počítačů.
* Verze jádra Linux pod 2.6.37 nepodporují NUMA v technologii Hyper-V s větší velikostí virtuálních počítačů. -Li tento problém ovlivňuje především starší distribuce pomocí hello proti proudu Red Hat 2.6.32 jádra která byla opravena v RHEL 6.6 (jádra 2.6.32 504). Systémy s operačním systémem vlastní jádra starší než 2.6.37 nebo na základě RHEL jádra starší, než se musí nastavit 2.6.32-504 hello parametr spouštěcího `numa=off` na příkazového řádku v grub.conf hello jádra. Další informace najdete v části Red Hat [KB 436883](https://access.redhat.com/solutions/436883).
* Nekonfigurujte přepnutí oddílu na disku operačního systému hello. Hello Linux agent může být nakonfigurované toocreate odkládací soubor na disku hello dočasných prostředků.  Další informace o této naleznete v následujících kroků hello.
* Všechny virtuální pevné disky hello musí mít velikostí, které jsou násobky 1 MB.

### <a name="installing-kernel-modules-without-hyper-v"></a>Instalace modulů jádra bez technologie Hyper-V
Azure je spuštěná na hypervisoru hello technologie Hyper-V, takže Linux vyžaduje, aby určité jádra moduly jsou nainstalovány v pořadí toorun v Azure. Pokud máte virtuální počítač, který se vytvořil mimo technologie Hyper-V, instalačních programů Linux hello nesmí obsahovat hello ovladače pro Hyper-V v hello počáteční disku paměti RAM (initrd nebo initramfs) Pokud zjistí, zda je spuštěna prostředí Hyper-V. Při použití tooprepare systému (tj. Virtualbox, KVM atd.) jiným virtualizačním bitové kopie systému Linux, musíte toorebuild hello initrd tooensure, který alespoň hello `hv_vmbus` a `hv_storvsc` jádra moduly jsou k dispozici na počáteční disku paměti RAM hello.  Jde o známý problém alespoň na systémy založené na nadřazený distribuční Red Hat hello.

Hello mechanismus pro nové sestavení hello initrd nebo initramfs image může lišit v závislosti na distribuční hello. Projděte si dokumentaci k vaší distribuční nebo podporu pro správné postup hello.  Tady je jeden příklad, jak toorebuild hello initrd pomocí hello `mkinitrd` nástroj:

Nejprve Zálohujte stávající image initrd hello:

    # cd /boot
    # sudo cp initrd-`uname -r`.img  initrd-`uname -r`.img.bak

V dalším kroku znovu sestavit hello initrd s hello `hv_vmbus` a `hv_storvsc` moduly jádra:

    # sudo mkinitrd --preload=hv_storvsc --preload=hv_vmbus -v -f initrd-`uname -r`.img `uname -r`


### <a name="resizing-vhds"></a>Změna velikosti virtuálních pevných disků
Image virtuálního pevného disku na Azure musí mít virtuální velikost zarovnán too1MB.  Obvykle virtuální pevné disky vytvořené pomocí technologie Hyper-V už zarovnání správně.  Pokud hello virtuálního pevného disku není zarovnána správně, může dostat k chybě zprávy podobné toohello následující pokusíte-li toocreate *image* z vaší virtuálního pevného disku:

    "hello VHD http://<mystorageaccount>.blob.core.windows.net/vhds/MyLinuxVM.vhd has an unsupported virtual size of 21475270656 bytes. hello size must be a whole number (in MBs).”

tooremedy toto můžete změnit velikost virtuálního počítače pomocí konzoly Správce technologie Hyper-V hello nebo hello hello [změny velikosti virtuálního pevného disku](http://technet.microsoft.com/library/hh848535.aspx) rutiny prostředí Powershell.  Pokud používáte v prostředí systému Windows se doporučuje toouse qemu img tooconvert (v případě potřeby) a změňte velikost hello virtuálního pevného disku.

> [!NOTE]
> Je známého problému v qemu img verze > = 2.2.1, jejímž výsledkem nesprávně naformátovaný VHD. ve verzi 2.6 QEMU byl opraven problém Hello. Je doporučeno toouse qemu-img 2.2.0 nebo nižší nebo aktualizace too2.6 nebo vyšší. Referenční dokumentace: https://bugs.launchpad.net/qemu/+bug/1490611.
> 
> 

1. Změna velikosti virtuálního pevného disku přímo pomocí nástrojů, jako hello `qemu-img` nebo `vbox-manage` může mít za následek nelze spustit VHD.  Proto se doporučuje toofirst převést hello image NEZPRACOVANÁ disku tooa virtuálního pevného disku.  Pokud hello image virtuálního počítače byla již vytvořena jako image NEZPRACOVANÁ disku (hello výchozí nastavení pro některé hypervisory například KVM) může tento krok přeskočit:
   
       # qemu-img convert -f vpc -O raw MyLinuxVM.vhd MyLinuxVM.raw

2. Vypočítejte velikost hello tooensure bitové kopie disku, který hello virtuální velikost je zarovnaný too1MB požadované hello.  s tím pomůže Hello následující skript prostředí bash.  skript Hello používá "`qemu-img info`" toodetermine hello virtuální velikost bitové kopie disku hello a pak vypočítá velikost toohello hello další 1 MB:
   
       rawdisk="MyLinuxVM.raw"
       vhddisk="MyLinuxVM.vhd"
   
       MB=$((1024*1024))
       size=$(qemu-img info -f raw --output json "$rawdisk" | \
              gawk 'match($0, /"virtual-size": ([0-9]+),/, val) {print val[1]}')
   
       rounded_size=$((($size/$MB + 1)*$MB))
       echo "Rounded Size = $rounded_size"

3. Změna velikosti hello nezpracovaná disku pomocí $rounded_size jako sada v hello výše skriptu:
   
       # qemu-img resize MyLinuxVM.raw $rounded_size

4. Nyní, převést zpět tooa hello NEZPRACOVANÁ disku pevné velikosti virtuálního pevného disku:
   
       # qemu-img convert -f raw -o subformat=fixed -O vpc MyLinuxVM.raw MyLinuxVM.vhd

   Nebo s verzí qemu **2.6 +** zahrnují hello `force_size` možnost:

       # qemu-img convert -f raw -o subformat=fixed,force_size -O vpc MyLinuxVM.raw MyLinuxVM.vhd

## <a name="linux-kernel-requirements"></a>Požadavky na Linux jádra
Hello Linux integrační služby (LIS) ovladače pro Hyper-V a Azure jsou podílí přímo toohello nadřazeného Linux jádra. Velkém množství distribucí, které obsahují nejnovější verzi jádra Linux (tj. 3.x) bude již je k dispozici tyto ovladače, nebo v opačném případě zadejte přeneseny zpět verze těchto ovladačů s jejich jádra.  Tyto ovladače jsou neustále aktualizován v hello nadřazeného jádra s nové opravy a funkce, takže pokud je to možné, doporučujeme toorun [schválené distribuční](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) který bude obsahovat tyto opravy a aktualizace.

Pokud používáte hodnotu typu variant Red Hat Enterprise Linux verze **6.0 6.3**, bude nutné tooinstall hello nejnovější ovladače LIS pro Hyper-V. Hello ovladače lze nalézt [v tomto umístění](http://go.microsoft.com/fwlink/p/?LinkID=254263&clcid=0x409). Od systému RHEL **6.4 +** (a odvozené konfigurace) ovladače hello LIS jsou již součástí hello jádra a proto žádné další instalační balíčky jsou potřebné toorun tyto systémy v Azure.

Pokud se vyžaduje vlastní jádra, je doporučeno toouse novější verzi jádra (tj. **3.8 +**). Pro tyto distribuce nebo dodavatelů, kteří spravují své vlastní jádra budou některé úsilí požadované tooregularly backport hello LIS ovladače z hello nadřazeného jádra tooyour vlastní jádra.  I když už používáte relativně nejnovější verzi jádra, doporučujeme, že tookeep sledování všech proti proudu opravy v hello LIS ovladače a backport ty podle potřeby. Hello umístění zdrojových souborů ovladačů LIS hello je k dispozici v hello [údržby programu](https://git.kernel.org/cgit/linux/kernel/git/torvalds/linux.git/tree/MAINTAINERS) soubor ve stromu zdroje jádra Linux hello:

    F:    arch/x86/include/asm/mshyperv.h
    F:    arch/x86/include/uapi/asm/hyperv.h
    F:    arch/x86/kernel/cpu/mshyperv.c
    F:    drivers/hid/hid-hyperv.c
    F:    drivers/hv/
    F:    drivers/input/serio/hyperv-keyboard.c
    F:    drivers/net/hyperv/
    F:    drivers/scsi/storvsc_drv.c
    F:    drivers/video/fbdev/hyperv_fb.c
    F:    include/linux/hyperv.h
    F:    tools/hv/

Na velmi minimální hello absenci hello následující opravy měla být známa toocause problémy v Azure a tak tyto musí být součástí hello jádra. Tento seznam je rozhodně není vyčerpávající nebo úplný pro všechny distribuce:

* [ata_piix: ve výchozím nastavení odložené ovladače disky toohello technologie Hyper-V](https://git.kernel.org/cgit/linux/kernel/git/torvalds/linux.git/commit/drivers/ata/ata_piix.c?id=cd006086fa5d91414d8ff9ff2b78fbb593878e3c)
* [miniport storvsc: účet pro pakety na cestě v cestě RESETOVÁNÍ hello](https://git.kernel.org/cgit/linux/kernel/git/torvalds/linux.git/commit/drivers/scsi/storvsc_drv.c?id=5c1b10ab7f93d24f29b5630286e323d1c5802d5c)
* [miniport storvsc: Vyhněte se použití WRITE_SAME](https://git.kernel.org/cgit/linux/kernel/git/next/linux-next.git/commit/drivers/scsi/storvsc_drv.c?id=3e8f4f4065901c8dfc51407e1984495e1748c090)
* [miniport storvsc: zakázat zápis stejné diskového pole RAID a ovladače adaptéru virtuální hostitel](https://git.kernel.org/cgit/linux/kernel/git/next/linux-next.git/commit/drivers/scsi/storvsc_drv.c?id=54b2b50c20a61b51199bedb6e5d2f8ec2568fb43)
* [miniport storvsc: oprava zrušení ukazatele NULL.](https://git.kernel.org/cgit/linux/kernel/git/next/linux-next.git/commit/drivers/scsi/storvsc_drv.c?id=b12bb60d6c350b348a4e1460cd68f97ccae9822e)
* [miniport storvsc: selhání prstenec vyrovnávací paměti může vést k zablokování vstupně-výstupních operací](https://git.kernel.org/cgit/linux/kernel/git/next/linux-next.git/commit/drivers/scsi/storvsc_drv.c?id=e86fb5e8ab95f10ec5f2e9430119d5d35020c951)
* [scsi_sysfs: ochranu proti dvojité provádění __scsi_remove_device](https://git.kernel.org/cgit/linux/kernel/git/next/linux-next.git/commit/drivers/scsi/scsi_sysfs.c?id=be821fd8e62765de43cc4f0e2db363d0e30a7e9b)

## <a name="hello-azure-linux-agent"></a>Hello Azure Linux Agent
Hello [Azure Linux Agent](../windows/agent-user-guide.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) (příkaz waagent) se vyžaduje tooproperly zřídit virtuální počítač s Linuxem v Azure. Můžete získat nejnovější verzi hello, soubor problémy nebo odeslání žádosti o přijetí změn v hello [úložiště GitHub agenta Linux](https://github.com/Azure/WALinuxAgent).

* v rámci Apache 2.0 hello licence vydání Hello Linux agent. Velkém množství distribucí již zadejte ot. / min nebo bázi deb balíčky pro hello agenta, a proto v některých případech to lze nainstalovat a aktualizovat s malým množstvím úsilí.
* Hello Azure Linux Agent vyžaduje Python v2.6 +.
* Hello agent také vyžaduje modul python pyasn1 hello. Většina distribuce to zadejte jako samostatný balíček, který může být instalován.
* V některých případech nemusí být kompatibilní s NetworkManager hello Azure Linux Agent. Mnoho balíčků RPM/bázi Deb hello poskytované distribuce nakonfigurujte NetworkManager jako balíček příkaz waagent toohello konflikt a proto odinstaluje NetworkManager při instalaci balíčku agenta Linux hello.

## <a name="general-linux-system-requirements"></a>Požadavky na systém Linux obecné

* Upravte hello jádra spouštěcí řádku GRUB nebo GRUB2 tooinclude hello následující parametry. To také zajistí, všechny zprávy konzoly jsou odesílány toohello první sériového portu, který může být užitečné Azure podporu pro ladění problémů:
  
        console=ttyS0,115200n8 earlyprintk=ttyS0,115200 rootdelay=300
  
    To také zajistí, všechny zprávy konzoly jsou odesílány toohello první sériového portu, který může být užitečné Azure podporu pro ladění problémů.
  
    Kromě výše uvedených toohello, se doporučuje příliš*odebrat* hello následující parametry, pokud existují:
  
        rhgb quiet crashkernel=auto
  
    Grafické a quiet spouštěcí nejsou užitečné při cloudovém prostředí, kde chceme, aby všechny protokoly toobe hello odeslané toohello sériového portu. Hello `crashkernel` možnost může být vlevo nakonfigurované v případě potřeby, ale Všimněte si, že tento parametr se sníží hello množství dostupné paměti v hello virtuální počítač podle 128 MB nebo víc, což může být problematické hello menší velikostí virtuálního počítače.

* Instalace hello Azure Linux Agent
  
    Hello Azure Linux Agent je požadované pro zřizování Linux image na platformě Azure.  Velkém množství distribucí zadejte hello agenta jako balíček RPM nebo bázi Deb (hello balíčku se obvykle označuje jako 'WALinuxAgent' nebo 'walinuxagent').  Hello lze také nainstalovat agenta ručně pomocí následujících kroků hello v hello [Linux Agent průvodce](../windows/agent-user-guide.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

* Ujistěte se, že server SSH hello je nainstalován a nakonfigurován toostart při spuštění.  Je to obvykle výchozí hello.

* Nevytvářejte odkládacího prostoru na disku operačního systému hello
  
    Hello Azure Linux Agent mohou automaticky konfigurovat odkládacího souboru pomocí disku hello místní prostředek, který je připojený toohello virtuálních počítačů po zřízení v Azure. Všimněte si, je tento disk hello místní prostředek *dočasné* na disku a může být vyprázdnit po deprovisioned hello virtuálních počítačů. Po instalaci hello Azure Linux Agent (viz předchozí krok), upravte hello následující parametry v /etc/waagent.conf odpovídajícím způsobem:
  
        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this toowhatever you need it toobe.

* V posledním kroku spusťte následující příkazy toodeprovision hello virtuálního počítače hello:
  
        # sudo waagent -force -deprovision
        # export HISTSIZE=0
        # logout
  
  > [!NOTE]
  > Na Virtualbox mohou se zobrazit následující chyba po spuštění hello se příkaz waagent-force - deprovision.: `[Errno 5] Input/output error`. Tato chybová zpráva není příliš důležitý a můžete ignorovat.
  > 
  > 

* Pak bude nutné tooshut dolů hello virtuálního počítače a nahrajte tooAzure hello virtuálního pevného disku.

