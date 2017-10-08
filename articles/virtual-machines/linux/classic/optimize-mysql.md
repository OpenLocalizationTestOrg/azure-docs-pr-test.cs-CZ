---
title: "aaaOptimize MySQL výkonu v systému Linux | Microsoft Docs"
description: "Zjistěte, jak toooptimize MySQL spuštěna na virtuálním počítači Azure (VM) s Linuxem."
services: virtual-machines-linux
documentationcenter: 
author: NingKuang
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 0c1c7fc5-a528-4d84-b65d-2df225f2233f
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 05/31/2017
ms.author: ningk
ms.openlocfilehash: 9e6458723233721e06f30b9de33635d403eefcba
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="optimize-mysql-performance-on-azure-linux-vms"></a>Optimalizace výkonu databáze MySQL na virtuálních počítačích Azure Linux
Existuje celá řada faktorů, které ovlivňují výkon databáze MySQL na Azure, jak v výběr virtuální hardwarové a softwarové konfigurace. Tento článek se zaměřuje na optimalizace výkonu úložiště, systému a konfigurace databáze.

> [!IMPORTANT]
> Azure má dva různé modely nasazení pro vytváření a práci s prostředky: [Azure Resource Manager](../../../resource-manager-deployment-model.md) a classic. Tento článek se zabývá pomocí modelu nasazení classic hello. Společnost Microsoft doporučuje, aby většina nových nasazení používala model Resource Manager hello. Informace o optimalizace virtuálního počítače s Linuxem pomocí modelu Resource Manager hello najdete v tématu [optimalizovat virtuálním počítačům s Linuxem v Azure](../optimization.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

## <a name="utilize-raid-on-an-azure-virtual-machine"></a>Využívat RAID na virtuální počítač Azure
Úložiště je hello klíčovým faktorem, který ovlivňuje výkon databáze v prostředí cloudu. Porovnání tooa jeden disk, RAID zajistí rychlejší přístup prostřednictvím souběžnosti. Další informace najdete v tématu [standardní RAID úrovně](http://en.wikipedia.org/wiki/Standard_RAID_levels).   

Propustnost vstupu/výstupu disku a vstupně-výstupních operací dobu odezvy v Azure je možné zlepšit prostřednictvím RAID. Naše testy testovacího prostředí zobrazit, může být dvojitá propustnost vstupu/výstupu disku a doby odezvy vstupně-výstupních operací může snížit o polovinu v průměru při (z toofour dva, čtyři tooeight atd.) se zdvojnásobí hello počet disků RAID. V tématu [příloha A](#AppendixA) podrobnosti.  

Kromě toho toodisk vstupně-výstupních operací, MySQL výkonu zvyšuje, když zvýšíte úroveň pole RAID hello.  V tématu [příloha B](#AppendixB) podrobnosti.  

Také můžete chtít velikost bloku tooconsider hello. Obecně platí když máte větší velikost bloku, získáte nižší nároky, hlavně pro velké zápisy. Ale když hello velikost deduplikačního bloku dat je příliš velký, může přidat další režie, které zabraňují využívat výhod RAID. Hello aktuální výchozí velikost je 512 KB, který je ověřené toobe optimální pro nejobecnější provozní prostředí. V tématu [příloha C](#AppendixC) podrobnosti.   

Existují omezení na tom, kolik disků můžete přidat pro typy jiný virtuální počítač. Tato omezení jsou podrobně popsané na [velikosti virtuálního počítače a cloudové služby pro Azure](http://msdn.microsoft.com/library/azure/dn197896.aspx). I když můžete tooset až RAID s méně disky, budete potřebovat čtyři připojené disky toofollow hello RAID příklad dat v tomto článku.  

Tento článek předpokládá jste již vytvořili virtuální počítač s Linuxem a MYSQL nainstalován a nakonfigurován. Další informace o začátcích najdete v tématu Jak tooinstall MySQL v Azure.  

### <a name="set-up-raid-on-azure"></a>Nastavení diskového pole RAID na Azure
Hello následující kroky ukazují, jak toocreate RAID na platformě Azure pomocí hello portálu Azure. Můžete také nastavit tak RAID pomocí skriptů prostředí Windows PowerShell.
V tomto příkladu nakonfigurujeme RAID 0 s čtyři disky.  

#### <a name="add-a-data-disk-tooyour-virtual-machine"></a>Přidat datový disk tooyour virtuální počítač
V hello portálu Azure přejděte toohello řídicího panelu a vyberte toowhich hello virtuálního počítače chcete tooadd datový disk. V tomto příkladu je virtuální počítač hello mysqlnode1.  

<!--![Virtual machines][1]-->

Klikněte na tlačítko **disky** a pak klikněte na **připojit nový**.

![Virtuální počítače přidejte disk](media/optimize-mysql/virtual-machines-linux-optimize-mysql-perf-Disks-option.png)

Vytvoření nového disku 500 GB. Ujistěte se, že **předvoleb mezipaměti hostitele** je nastaven příliš**žádné**.  Až budete hotovi, klikněte na tlačítko **OK**.

![Připojte prázdný disk](media/optimize-mysql/virtual-machines-linux-optimize-mysql-perf-attach-empty-disk.png)


Tento postup přidá jeden prázdný disk do virtuálního počítače. Opakujte tento krok tři vícekrát, aby měli čtyři datových disků RAID.  

Můžete zobrazit hello přidat jednotky ve virtuálním počítači hello prohlížením hello jádra zprávu protokolu. Například toosee to na Ubuntu hello použijte následující příkaz:  

    sudo grep SCSI /var/log/dmesg

#### <a name="create-raid-with-hello-additional-disks"></a>Vytvoření RAID s hello dalších disků.
Hello následující kroky popisují, jak příliš[konfigurace softwaru diskového pole RAID v systému Linux](../configure-raid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

> [!NOTE]
> Pokud používáte systém souborů XFS hello, provést následující kroky po vytvoření RAID hello.
>
>

tooinstall XFS Debian a Ubuntu, máta Linux hello použijte následující příkaz:  

    apt-get -y install xfsprogs  

tooinstall XFS Fedora, CentOS nebo RHEL, hello použijte následující příkaz:  

    yum -y install xfsprogs  xfsdump


#### <a name="set-up-a-new-storage-path"></a>Nastavit novou cestu úložiště
Použijte následující příkaz tooset si novou cestu úložiště hello:  

    root@mysqlnode1:~# mkdir -p /RAID0/mysql

#### <a name="copy-hello-original-data-toohello-new-storage-path"></a>Zkopírujte hello původní data toohello novou cestu k úložišti
Použijte následující příkaz toocopy toohello nové úložiště cestu k datům hello:  

    root@mysqlnode1:~# cp -rp /var/lib/mysql/* /RAID0/mysql/

#### <a name="modify-permissions-so-mysql-can-access-read-and-write-hello-data-disk"></a>Upravit oprávnění, můžete přístup MySQL (čtení a zápisu) hello datový disk
Použijte následující příkaz toomodify oprávnění hello:  

    root@mysqlnode1:~# chown -R mysql.mysql /RAID0/mysql && chmod -R 755 /RAID0/mysql


## <a name="adjust-hello-disk-io-scheduling-algorithm"></a>Upravit v/v disku hello plánování algoritmus
Linux implementuje čtyři typy vstupně-výstupních operací plánování algoritmů:  

* Nedojde k žádné akci algoritmus (ne operace)
* Algoritmus konečného termínu (termín)
* Úplně správného algoritmu front zpráv (CFQ)
* Nároky období algoritmus (Anticipatory)  

Můžete vybrat jiný plánovače vstupně-výstupních operací v různých scénářích toooptimize výkonu. V prostředí s úplně náhodný přístup není velký rozdíl mezi hello CFQ a algoritmy konečný termín pro výkon. Doporučujeme, abyste nastavili hello MySQL database prostředí tooDeadline pro stabilitu. Pokud existuje mnoho sekvenčních vstupně-výstupních operací, CFQ může snížit výkon vstupně-výstupní operace disku.   

Pro SSD a dalších zařízení nedojde k žádné akci nebo konečný termín můžete dosáhnout lepší výkon než Plánovač výchozí hello.   

Předchozí toohello jádra 2.5, vstupně-výstupní výchozí hello plánování algoritmus je konečný termín. Počínaje hello jádra 2.6.18, CFQ stala hello výchozí algoritmus plánování vstupně-výstupní operace.  Můžete určit toto nastavení při spuštění jádra nebo dynamicky toto nastavení změnit, když je spuštěn systém hello.  

Hello následující příklad ukazuje, jak toocheck a nastavte hello výchozí plánovač toohello nedojde k žádné akci algoritmus řady Debian distribuční hello.  

### <a name="view-hello-current-io-scheduler"></a>Zobrazení hello aktuálního vstupně-výstupních operací plánovače
tooview hello hello scheduler spusťte následující příkaz:  

    root@mysqlnode1:~# cat /sys/block/sda/queue/scheduler

Zobrazí se následující výstup, který označuje aktuálního plánovače hello:  

    noop [deadline] cfq


### <a name="change-hello-current-device-devsda-of-hello-io-scheduling-algorithm"></a>Změnit aktuální zařízení hello (/ dev/sda) plánování algoritmu hello vstupně-výstupních operací
Spusťte následující příkazy toochange hello aktuální zařízení hello:  

    azureuser@mysqlnode1:~$ sudo su -
    root@mysqlnode1:~# echo "noop" >/sys/block/sda/queue/scheduler
    root@mysqlnode1:~# sed -i 's/GRUB_CMDLINE_LINUX=""/GRUB_CMDLINE_LINUX_DEFAULT="quiet splash elevator=noop"/g' /etc/default/grub
    root@mysqlnode1:~# update-grub

> [!NOTE]
> Nastavení to samostatně/dev/sda není užitečné. Je nutné ji nastavit na všech discích data níž se nachází databáze hello.  
>
>

Měli byste vidět hello následující výstup, označující, že grub.cfg byla znovu sestavena úspěšně a že scheduler výchozí hello byl aktualizovaný tooNOOP:  

    Generating grub configuration file ...
    Found linux image: /boot/vmlinuz-3.13.0-34-generic
    Found initrd image: /boot/initrd.img-3.13.0-34-generic
    Found linux image: /boot/vmlinuz-3.13.0-32-generic
    Found initrd image: /boot/initrd.img-3.13.0-32-generic
    Found memtest86+ image: /memtest86+.elf
    Found memtest86+ image: /memtest86+.bin
    done

Pro hello Red Hat distribuční rodiny třeba jenom hello, následující příkaz:

    echo 'echo noop >/sys/block/sda/queue/scheduler' >> /etc/rc.local

## <a name="configure-system-file-operations-settings"></a>Konfigurace nastavení operace systému souborů
Jeden osvědčeným postupem je toodisable hello *atime* funkce protokolování v systému souborů hello. Atime je hello čas posledního přístupu souboru. Vždy, když je přístup k souboru, záznamů systému souboru hello hello časové razítko v protokolu hello. Tyto informace se ale zřídka používá. Ji můžete vypnout, pokud tomu tak není, které se sníží celkový čas přístup k disku.  

toodisable atime protokolování, můžete potřebovat toomodify hello souboru systému konfigurační soubor/etc / fstab a přidat hello **noatime** možnost.  

Můžete třeba upravte soubor /etc/fstab hello vim přidáním hello noatime, jak je znázorněno v následující ukázka hello:  

    # CLOUD_IMG: This file was created/modified by hello Cloud Image build process
    UUID=3cc98c06-d649-432d-81df-6dcd2a584d41       /        ext4   defaults,discard        0 0
    #Add hello “noatime” option below toodisable atime logging
    UUID="431b1e78-8226-43ec-9460-514a9adf060e"     /RAID0   xfs   defaults,nobootwait, noatime 0 0
    /dev/sdb1       /mnt    auto    defaults,nobootwait,comment=cloudconfig 0       2

Znovu připojte hello systém souborů s hello následující příkaz:  

    mount -o remount /RAID0

Test hello upravit výsledek. Když upravíte hello testovací soubor, čas přístupu hello se neaktualizuje. Dobrý den, následující příklady zobrazují, jaký kód hello vypadá před a po změnách.

Před:        

![Před úpravou přístupu kódu][5]

Po:

![Po změnách přístupu kódu][6]

## <a name="increase-hello-maximum-number-of-system-handles-for-high-concurrency"></a>Zvýšit maximální počet popisovačů systému pro vysokou souběžnosti hello
MySQL je vysoká souběžnosti databáze. Hello výchozí počet souběžných obslužných rutin je 1024 pro Linux, který není vždy dostatečná. Pomocí následujících kroků tooincrease hello maximální souběžných popisovačů systému hello systému toosupport vysoké souběžnosti z databáze MySQL hello.

### <a name="modify-hello-limitsconf-file"></a>Upravte soubor limits.conf hello
tooincrease hello maximální povolené souběžných obslužných rutin, přidejte následující čtyři řádků v souboru /etc/security/limits.conf hello hello. Všimněte si, že je 65536 hello maximální počet, který podporuje hello systému.   

    * logicky nofile 65536
    * pevné nofile 65536
    * logicky nproc 65536
    * pevné nproc 65536

### <a name="update-hello-system-for-hello-new-limits"></a>Aktualizovat hello systém nové omezení hello
tooupdate hello systému, spustit hello následující příkazy:  

    ulimit -SHn 65536
    ulimit -SHu 65536

### <a name="ensure-that-hello-limits-are-updated-at-boot-time"></a>Zajistěte, aby se při spuštění aktualizovala hello omezení
Uveďte hello následující příkazy spuštění v souboru /etc/rc.local hello tak projeví při spuštění.  

    echo “ulimit -SHn 65536” >>/etc/rc.local
    echo “ulimit -SHu 65536” >>/etc/rc.local

## <a name="mysql-database-optimization"></a>Optimalizace databáze MySQL
tooconfigure MySQL v Azure, můžete použít hello stejné strategie optimalizace výkonu můžete použít na místním počítači.  

Hello hlavní vstupně-výstupních operací optimalizace pravidel jsou:   

* Zvětšete velikost mezipaměti hello.
* Snížení doby odezvy vstupně-výstupní operace.  

nastavení serveru toooptimize MySQL, můžete aktualizovat hello my.cnf souboru, který je hello výchozí konfigurační soubor pro server a klientských počítačů.  

Hello následující položky konfigurace jsou hello hlavní faktory, které ovlivňují výkon MySQL:  

* **innodb_buffer_pool_size**: fondu vyrovnávací paměti hello obsahuje data ve vyrovnávací paměti a hello index. Obvykle je nastavena v procentech too70 fyzické paměti.
* **innodb_log_file_size**: Toto je velikost protokolu hello operaci znovu. Můžete použít tooensure protokoly opakování operace zápisu jsou rychlé, spolehlivé a použitelná pro obnovení po chybě. Toto nastavení too512 MB, který vám poskytne dostatek místa pro protokolování operace zápisu.
* **max_connections**: v některých případech aplikace neukončujte připojení správně. Větší hodnotu získáte hello server déle toorecycle nečinný připojení. Hello maximální počet připojení je 10 000, ale doporučuje hello, že maximální počet je 5 000.
* **Innodb_file_per_table**: Toto nastavení povolí nebo zakáže možnost hello InnoDB toostore tabulek v samostatné soubory. Zapněte tooensure hello možnost, že několik operací pokročilé správy může být použitá efektivně. Z výkonu hlediska může urychlit přenos místo tabulky hello a optimalizace výkonu správy zbytků hello. Hello doporučená nastavení pro tuto možnost je ON.</br></br>
Z databáze MySQL 5.6 hello výchozí nastavení je ON, takže není vyžadována žádná akce. U starších verzí hello výchozí nastavení je VYPNUTÝ. Hello parametr změnit před načtením dat, protože to ovlivňuje pouze nově vytvořené tabulky.
* **innodb_flush_log_at_trx_commit**: hello výchozí hodnota je 1, s hello nastavte obor too0 ~ 2. Hello výchozí hodnota je hello nejvíce vhodnou možností pro samostatné databáze MySQL. nastavení Hello 2 umožňuje hello většina integritu dat a je vhodný pro hlavní server v clusteru MySQL. nastavení Hello 0 umožňuje ztrátě dat, která může mít vliv na spolehlivost (v některých případech s lepším výkonem) a je vhodný pro podřízený v clusteru MySQL.
* **Innodb_log_buffer_size**: hello protokolu vyrovnávací paměti umožňuje transakce toorun bez nutnosti tooflush hello protokolu toodisk před potvrzení transakce hello. Pokud je binární rozsáhlý objekt nebo textové pole, mezipaměti hello využijí rychle a aktivuje se často diskové vstupně-výstupní operace. Je lépe zvýšit velikost vyrovnávací paměti hello, pokud není Innodb_log_waits proměnné stavu 0.
* **query_cache_size**: hello nejlepší možnost je toodisable z hello outset. Nastavit query_cache_size too0 (Toto je výchozí nastavení hello v MySQL 5.6) a použít jiné metody toospeed zpracování dotazů.  

V tématu [Dodatek D](#AppendixD) porovnání před a po hello optimalizace výkonu.

## <a name="turn-on-hello-mysql-slow-query-log-for-analyzing-hello-performance-bottleneck"></a>Zapnout protokol pomalé dotazu hello MySQL pro analýzu hello přetížení
protokol pomalé dotazu MySQL Hello můžete identifikovat hello pomalé dotazů pro databázi MySQL. Když povolíte protokol pomalé dotazu hello MySQL, můžete použít nástroje MySQL jako **mysqldumpslow** tooidentify hello přetížení.  

Ve výchozím nastavení to není povoleno. Zapnutí protokol pomalé dotazu hello můžou využívat některé prostředky procesoru. Doporučujeme, abyste povolili to dočasně pro řešení potíží s kritické body. tooturn na protokol hello pomalé dotazu:

1. Upravte soubor my.cnf hello přidáním následující řádky toohello end hello:

        long_query_time = 2
        slow_query_log = 1
        slow_query_log_file = /RAID0/mysql/mysql-slow.log

2. Restartujte server, MySQL hello.

        service  mysql  restart

3. Zkontrolujte, zda text hello nastavení trvá vliv pomocí hello **zobrazit** příkaz.

![ON zpomalit protokol dotazu][7]   

![Výsledky zpomalit protokol dotazu][8]

V tomto příkladu vidíte, že tato funkce pomalé dotazu hello je zapnutý. Pak můžete použít hello **mysqldumpslow** nástroj kritické body toodetermine a optimalizace výkonu, jako je například přidávání indexy.

## <a name="appendices"></a>Přílohy
Hello následují ukázková výkonu testovací data vytvořeného v cílové testovacím prostředí. Poskytují obecné na trend data výkonu hello s jinou ladění přístupy výkonu. výsledky Hello se mohou lišit v rámci různých verzí prostředí nebo produktu.

### <a name="AppendixA"></a>Příloha A  
**Výkon disku (IOPS) s různými úrovněmi diskového pole RAID**

![Disk IOPS s různými úrovněmi diskového pole RAID][9]

**Test příkazy**  

    fio -filename=/path/test -iodepth=64 -ioengine=libaio -direct=1 -rw=randwrite -bs=4k -size=5G -numjobs=64 -runtime=30 -group_reporting -name=test-randwrite

> [!NOTE]
> Hello úlohy, které obsahují tento test používá 64 vláken, pokusu o tooreach hello horní limit počtu RAID.
>
>

### <a name="AppendixB"></a>Dodatek B  
**Porovnání výkonu (propustnost) MySQL s různými úrovněmi diskového pole RAID**   
(Systém souborů XFS)

![Porovnání výkonu MySQL s různými úrovněmi diskového pole RAID][10]  
![Porovnání výkonu MySQL s různými úrovněmi diskového pole RAID][11]

**Test příkazy**

    mysqlslap -p0ps.123 --concurrency=2 --iterations=1 --number-int-cols=10 --number-char-cols=10 -a --auto-generate-sql-guid-primary --number-of-queries=10000 --auto-generate-sql-load-type=write –engine=innodb

**Porovnání výkonu (OLTP) MySQL s různými úrovněmi diskového pole RAID**  
![Porovnání výkonu (OLTP) MySQL s různými úrovněmi diskového pole RAID][12]

**Test příkazy**

    time sysbench --test=oltp --db-driver=mysql --mysql-user=root --mysql-password=0ps.123  --mysql-table-engine=innodb --mysql-host=127.0.0.1 --mysql-port=3306 --mysql-socket=/var/run/mysqld/mysqld.sock --mysql-db=test --oltp-table-size=1000000 prepare

### <a name="AppendixC"></a>Příloha C   
**Porovnání výkonu (IOPS) disku pro různé bloku velikosti**  
(Systém souborů XFS)

![][13]

**Test příkazy**  

    fio -filename=/path/test -iodepth=64 -ioengine=libaio -direct=1 -rw=randwrite -bs=4k -size=30G -numjobs=64 -runtime=30 -group_reporting -name=test-randwrite
    fio -filename=/path/test -iodepth=64 -ioengine=libaio -direct=1 -rw=randwrite -bs=4k -size=1G -numjobs=64 -runtime=30 -group_reporting -name=test-randwrite  

použít pro toto testování velikosti souborů Hello 30 GB 1 GB, v uvedeném pořadí a s RAID 0 (4 disky) XFS systému souborů.

### <a name="AppendixD"></a>Dodatek D  
**Porovnání výkonu (propustnost) MySQL před a po optimalizace**  
(Systém souborů XFS)

![Porovnání výkonu (propustnost) MySQL před a po optimalizace][14]

**Test příkazy**

    mysqlslap -p0ps.123 --concurrency=2 --iterations=1 --number-int-cols=10 --number-char-cols=10 -a --auto-generate-sql-guid-primary --number-of-queries=10000 --auto-generate-sql-load-type=write –engine=innodb,misam

**nastavení konfigurace Hello výchozí a optimalizace vypadá takto:**

| Parametry | Výchozí | Optimalizace |
| --- | --- | --- |
| **innodb_buffer_pool_size** |Žádný |7 GB |
| **innodb_log_file_size** |5 MB. |512 MB |
| **max_connections** |100 |5000 |
| **innodb_file_per_table** |0 |1 |
| **innodb_flush_log_at_trx_commit** |1 |2 |
| **innodb_log_buffer_size** |8 MB |128 MB |
| **query_cache_size** |16 MB |0 |

Další podrobné [parametry konfigurace optimalizace](http://dev.mysql.com/doc/refman/5.6/en/innodb-configuration.html), najdete v toohello [MySQL oficiální pokyny](http://dev.mysql.com/doc/refman/5.6/en/innodb-parameters.html#sysvar_innodb_flush_method).  

  **Testovací prostředí**  

| Hardware | Podrobnosti |
| --- | --- |
| Procesor |AMD Opteron(tm) procesoru 4171 HE / 4 jádra |
| Memory (Paměť) |14 GB |
| Disk |10 GB místa na disku |
| Operační systém |Ubuntu 14.04.1 LTS |

[1]:media/optimize-mysql/virtual-machines-linux-optimize-mysql-perf-01.png
[2]:media/optimize-mysql/virtual-machines-linux-optimize-mysql-perf-02.png
[3]:media/optimize-mysql/virtual-machines-linux-optimize-mysql-perf-03.png
[4]:media/optimize-mysql/virtual-machines-linux-optimize-mysql-perf-04.png
[5]:media/optimize-mysql/virtual-machines-linux-optimize-mysql-perf-05.png
[6]:media/optimize-mysql/virtual-machines-linux-optimize-mysql-perf-06.png
[7]:media/optimize-mysql/virtual-machines-linux-optimize-mysql-perf-07.png
[8]:media/optimize-mysql/virtual-machines-linux-optimize-mysql-perf-08.png
[9]:media/optimize-mysql/virtual-machines-linux-optimize-mysql-perf-09.png
[10]:media/optimize-mysql/virtual-machines-linux-optimize-mysql-perf-10.png
[11]:media/optimize-mysql/virtual-machines-linux-optimize-mysql-perf-11.png
[12]:media/optimize-mysql/virtual-machines-linux-optimize-mysql-perf-12.png
[13]:media/optimize-mysql/virtual-machines-linux-optimize-mysql-perf-13.png
[14]:media/optimize-mysql/virtual-machines-linux-optimize-mysql-perf-14.png

