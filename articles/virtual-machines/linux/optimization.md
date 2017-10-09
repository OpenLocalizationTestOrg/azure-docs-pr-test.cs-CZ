---
title: "aaaOptimize virtuálním počítačům s Linuxem v Azure | Microsoft Docs"
description: "Další tipy toomake některé optimalizace se, že jste nastavili virtuálním počítačům s Linuxem pro optimální výkon na Azure"
keywords: "virtuální počítač Linux, virtuální počítač linux, ubuntu virtuálního počítače"
services: virtual-machines-linux
documentationcenter: 
author: rickstercdn
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 8baa30c8-d40e-41ac-93d0-74e96fe18d4c
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 09/06/2016
ms.author: rclaus
ms.openlocfilehash: 89a9ac022928a2801a9a15e1c172340352745354
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="optimize-your-linux-vm-on-azure"></a>Optimalizace virtuálního počítače s Linuxem v Azure
Vytvoření virtuálního počítače (VM) Linux je snadno toodo z hello příkazového řádku nebo z portálu hello. Tento kurz ukazuje, jak tooensure jste nastavili ho toooptimize jeho výkon na platformě Microsoft Azure hello. Toto téma používá virtuálního počítače s Ubuntu Server, ale můžete vytvořit také pomocí virtuálních počítačů Linux [vlastní Image jako šablona](create-upload-generic.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).  

## <a name="prerequisites"></a>Požadavky
Toto téma předpokládá, že již máte funkční předplatné Azure ([bezplatné zkušební verze registrace](https://azure.microsoft.com/pricing/free-trial/)) a již zřídit virtuální počítač do vašeho předplatného Azure. Ujistěte se, že máte hello nejnovější [Azure CLI 2.0](/cli/azure/install-az-cli2) nainstalován a přihlášení tooyour předplatné s [az přihlášení](/cli/azure/#login) před [vytvoření virtuálního počítače](quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

## <a name="azure-os-disk"></a>Azure Disk operačního systému
Po vytvoření virtuálního počítače s Linuxem v Azure, má dva disky, které jsou s ním spojená. **/ dev/sda** je disk operačního systému, **/dev/sdb** je dočasným diskovým.  Nepoužívejte hello hlavní disk operačního systému (**/dev/sda**) pro všechno, co s výjimkou hello operačního systému, protože je optimalizovaná pro rychlé spuštění virtuálního počítače a neposkytuje dobrý výkon pro zatížení. Chcete, aby tooattach jeden nebo více disků tooyour úložiště virtuálního počítače tooget trvalé a optimalizované pro data. 

## <a name="adding-disks-for-size-and-performance-targets"></a>Přidání disků pro velikost a cílech výkonnosti
Podle hello velikost virtuálního počítače, můžete připojit až too16 další disky na A-Series, 32 disky na D-Series a 64 disků na počítači G-Series - každý až velikost too1 TB. Podle potřeby na místa a požadavky na IOps přidáte další disky. Každý disk má výkonu cílem 500 IOps pro Standard Storage nebo vyšší too5000 IOps na disku pro Storage úrovně Premium.  Další informace o discích Premium Storage najdete v tématu [úložiště Premium: vysoce výkonné úložiště pro virtuální počítače Azure](../../storage/common/storage-premium-storage.md)

tooachieve hello nejvyšší IOps na discích úložiště Premium, kde jejich mezipaměti nastavení tooeither **jen pro čtení** nebo **žádné**, je nutné zakázat **překážek** při připojení hello systému souborů v systému Linux. Není nutné překážek protože hello zapíše tooPremium úložiště zálohovány disky jsou odolné, aby se tato nastavení mezipaměti.

* Pokud používáte **reiserFS**, zakažte překážek pomocí možnosti připojení hello `barrier=none` (pro povolení překážek, použijte `barrier=flush`)
* Pokud používáte **ext3/ext4**, zakažte překážek pomocí možnosti připojení hello `barrier=0` (pro povolení překážek, použijte `barrier=1`)
* Pokud používáte **XFS**, zakažte překážek pomocí možnosti připojení hello `nobarrier` (pro povolení překážek, použijte možnost hello `barrier`)

## <a name="unmanaged-storage-account-considerations"></a>Důležité informace o účtu nespravovaného úložiště
Hello výchozí akce při vytvoření virtuálního počítače s hello Azure CLI 2.0 je toouse Azure spravované disky.  Tyto disky jsou zpracovávány hello platformy Azure a nevyžadují, aby všechny přípravné nebo umístění toostore je.  Nespravované disky se vyžaduje účet úložiště a mají některé další výkonem.  Další informace o spravovaných discích najdete v tématu [Přehled služby Azure Managed Disks](../windows/managed-disks-overview.md).  Hello následující část popisuje faktory ovlivňující výkon jenom když použijete nespravované disky.  Znovu hello výchozí a doporučenou úložiště řešení je toouse spravované disky.

Pokud vytvoříte virtuální počítač s nespravované disky, ujistěte se, připojit disky z účtů úložiště umístěných v hello stejné oblasti jako váš počítač tooensure zavřete blízkosti a minimalizaci latence sítě.  Každý účet standardního úložiště má maximálně 20 tisíc IOps a kapacitou velikost 500 TB.  Tento limit funguje tooapproximately 40 zatížen disky, včetně disku hello operačního systému a všech datových disků, které vytvoříte. Pro účty služby Premium Storage neexistuje žádné omezení maximální IOps, ale existuje omezení velikosti 32 TB. 

Při práci s vysoké zatížení IOps a vybrali standardní úložiště pro disky, bude pravděpodobně nutné toosplit hello disky napříč více účtů úložiště toomake se, že nebyly dosáhl hello 20 000 IOps omezení pro účty úložiště Standard Storage. Virtuální počítač může obsahovat kombinaci disků z napříč jiným účtům úložiště a tooachieve typy účtu úložiště optimální konfiguraci.
 

## <a name="your-vm-temporary-drive"></a>Virtuální počítač dočasné jednotce
Ve výchozím nastavení při vytváření virtuálního počítače, Azure poskytuje disk s operačním systémem (**/dev/sda**) a dočasným diskovým (**/dev/sdb**).  Všechny další disky přidat zobrazit si jako **/dev/sdc**, **/dev/sdd**, **/dev/sde** a tak dále. Všechna data na dočasné disku (**/dev/sdb**) není odolné a může dojít ke ztrátě, pokud konkrétní události, například změna velikosti virtuálních počítačů, opakované nasazení, nebo údržby vynutí restartování virtuálního počítače.  Hello velikost a typ dočasné disku je související toohello velikost virtuálního počítače, které jste zvolili v době nasazení. Všechny hello premium velikosti virtuálních počítačů (řady DS, G a DS_V2) hello dočasné jednotce jsou zajišťované místní SSD pro další výkonu nahoru too48k IOps. 

## <a name="linux-swap-file"></a>Linux odkládací soubor
Pokud virtuální počítač Azure z image Ubuntu nebo CoreOS, můžete použít CustomData toosend cloudu config toocloud-init. Pokud jste [nahrát vlastní image Linux](upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) init cloudu, který používá, je také nakonfigurovat oddíly odkládacího souboru v cloudu init.

Ubuntu cloudu Image musíte použít cloudové init tooconfigure hello přepnutí oddílu. Další informace najdete v tématu [AzureSwapPartitions](https://wiki.ubuntu.com/AzureSwapPartitions).

Pro Image bez podpory cloudu init být Image virtuálních počítačů z Azure Marketplace hello nasazena Agent virtuálních počítačů Linux integrovat hello operačního systému. Tento agent umožňuje hello toointeract virtuálních počítačů s různými službami Azure. Za předpokladu, že jste nasadili standardní bitové kopie z hello Azure Marketplace, potřebovali byste toodo hello následující toocorrectly konfigurace Linux odkládací soubor:

Vyhledejte a upravit dvě položky v hello **/etc/waagent.conf** souboru. Tím i určovat hello existenci vyhrazené odkládací soubor a velikost odkládacího souboru hello. Parametry Hello hledáte toomodify jsou `ResourceDisk.EnableSwap=N` a`ResourceDisk.SwapSizeMB=0` 

Změna toohello parametry hello následující nastavení:

* ResourceDisk.EnableSwap=Y
* ResourceDisk.SwapSizeMB={size v MB toomeet vašim potřebám} 

Po provedení hello změnit, je nutné příkaz waagent hello toorestart nebo restartovat váš virtuální počítač s Linuxem tooreflect tyto změny.  Víte, byly implementovány změny hello a odkládací soubor byl vytvořen při použití hello `free` příkaz tooview volného místa. Hello následující příklad je 512MB odkládací soubor vytvořeny v důsledku změny hello **waagent.conf** souboru:

```bash
azuseruser@myVM:~$ free
            total       used       free     shared    buffers     cached
Mem:       3525156     804168    2720988        408       8428     633192
-/+ buffers/cache:     162548    3362608
Swap:       524284          0     524284
```

## <a name="io-scheduling-algorithm-for-premium-storage"></a>Plánování algoritmus vstupně-výstupních operací pro Storage úrovně Premium
S hello 2.6.18 Linux jádra hello výchozí vstupně-výstupních operací plánování algoritmus změnila z konečného termínu tooCFQ (algoritmus úplně správného řízení front). Pro vzory vstupů/výstupů náhodný přístup je nepatrné rozdíl ve výkonu rozdíly mezi CFQ a konečným termínem.  Pro založená na SSD disky, kde je vzor vstupně-výstupních operací disku hello převážně sekvenční, přepnout zpět toohello nedojde k žádné akci nebo termín algoritmus lepšího výkonu můžete dosáhnout vstupně-výstupní operace.

### <a name="view-hello-current-io-scheduler"></a>Zobrazení hello aktuálního vstupně-výstupních operací plánovače
Hello použijte následující příkaz:  

```bash
cat /sys/block/sda/queue/scheduler
```

Zobrazí následující výstup, který označuje hello aktuálního plánovače.  

```bash
noop [deadline] cfq
```

### <a name="change-hello-current-device-devsda-of-io-scheduling-algorithm"></a>Změnit aktuální zařízení hello (/ dev/sda) plánování algoritmus vstupně-výstupních operací
Hello použijte následující příkazy:  

```bash
azureuser@myVM:~$ sudo su -
root@myVM:~# echo "noop" >/sys/block/sda/queue/scheduler
root@myVM:~# sed -i 's/GRUB_CMDLINE_LINUX=""/GRUB_CMDLINE_LINUX_DEFAULT="quiet splash elevator=noop"/g' /etc/default/grub
root@myVM:~# update-grub
```

> [!NOTE]
> Toto nastavení pro použití **/dev/sda** samostatně není užitečné. Nastavte na všech datových disků, kde sekvenčních vstupně-výstupních operací dominuje vzor hello vstupně-výstupních operací.  

Měli byste vidět hello následující výstup, což indikuje, že **grub.cfg** byla znovu sestavena úspěšně a že hello výchozí plánovač byla aktualizovaná tooNOOP.  

```bash
Generating grub configuration file ...
Found linux image: /boot/vmlinuz-3.13.0-34-generic
Found initrd image: /boot/initrd.img-3.13.0-34-generic
Found linux image: /boot/vmlinuz-3.13.0-32-generic
Found initrd image: /boot/initrd.img-3.13.0-32-generic
Found memtest86+ image: /memtest86+.elf
Found memtest86+ image: /memtest86+.bin
done
```

Pro hello Redhat distribuční rodiny stačí pouze hello následující příkaz:   

```bash
echo 'echo noop >/sys/block/sda/queue/scheduler' >> /etc/rc.local
```

## <a name="using-software-raid-tooachieve-higher-iops"></a>Pomocí softwaru diskového pole RAID tooachieve vyšší I / Ops
Pokud vaše úlohy vyžadují více procesorů, než může poskytnout jeden disk, je třeba toouse konfiguraci RAID softwaru několik disků. Protože Azure již provádí odolnosti disku ve vrstvě hello místní infrastruktury, můžete dosáhnout hello nejvyšší úroveň výkonu z proložení konfigurace RAID-0.  Zřídit a disky v hello prostředí Azure a vytvořte tooyour virtuálního počítače s Linuxem před rozdělení do oddílů, formátování a připojení hello jednotky.  Další informace o konfiguraci instalace softwaru diskového pole RAID na virtuálním počítačům s Linuxem v azure naleznete v hello  **[konfigurace RAID softwaru v systému Linux](configure-raid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**  dokumentu.

## <a name="next-steps"></a>Další kroky
Pamatujte si, že všechny optimalizace diskuse, bude nutné tooperform testy před a po každé změny toomeasure hello dopad hello změnu má.  Optimalizace je krok za krokem proces, který má odlišné výsledky v různých počítačích ve vašem prostředí.  Co funguje pro jednu konfiguraci nemusí fungovat pro ostatní.

Některé prostředky tooadditional užitečné odkazy: 

* [Storage úrovně Premium: Vysoce výkonné úložiště pro úlohy virtuálních počítačů Azure](../../storage/common/storage-premium-storage.md)
* [Uživatelská příručka k Azure Linux Agent](../windows/agent-user-guide.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Optimalizace výkonu databáze MySQL na virtuálních počítačích Azure Linux](classic/optimize-mysql.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)
* [Konfigurace softwaru diskového pole RAID v systému Linux](configure-raid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

