---
title: "aaaDesign a implementujte Oracle databáze v Azure | Microsoft Docs"
description: "Návrh a implementaci k databázi Oracle v prostředí Azure."
services: virtual-machines-linux
documentationcenter: virtual-machines
author: v-shiuma
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 6/22/2017
ms.author: rclaus
ms.openlocfilehash: 8fa1207458695df1c7330ec626888b1b6b8d8939
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="design-and-implement-an-oracle-database-in-azure"></a>Návrh a implementaci k databázi Oracle v Azure

## <a name="assumptions"></a>Předpoklady

- Plánování toomigrate z tooAzure místní databáze Oracle.
- Pochopení hello máte v sestavách Oracle AWR různé metriky.
- Máte základní znalosti aplikace výkonu a využití platformy.

## <a name="goals"></a>Cíle

- Pochopit, jak toooptimize Oracle nasazení v Azure.
- Prozkoumejte možností pro databázi Oracle v prostředí Azure ladění výkonu.

## <a name="hello-differences-between-an-on-premises-and-azure-implementation"></a>Hello rozdíly mezi místním a Azure implementace 

Toto jsou některé důležité věci tookeep na paměti, když se migraci místní tooAzure aplikace. 

Jeden důležitý rozdíl je, že v implementaci Azure prostředkům, například virtuálních počítačů, disků a virtuální sítě sdílejí mezi ostatní klienty. Kromě toho prostředky může být omezena na základě požadavků hello. Místo zaměřené na zamezení selhání provozu (MTBF), Azure zaměřují na zbývajících selhání hello (MTTR).

Hello následující tabulka uvádí některé z hello rozdíly mezi místními implementace a implementaci Azure pro databázi Oracle.

> 
> |  | **Místní implementace** | **Azure implementace** |
> | --- | --- | --- |
> | **Sítě** |LAN NEBO WAN  |SDN (softwarově definované sítě)|
> | **Skupina zabezpečení** |Nástroje pro omezení adresy IP a portu |[Skupina zabezpečení sítě (NSG)](https://azure.microsoft.com/blog/network-security-groups) |
> | **Odolnost proti** |MTBF (střední čas mezi selhání) |MTTR (toorecovery střední čas)|
> | **Plánovaná údržba** |Opravy chyb a upgrady|[Skupiny dostupnosti](https://docs.microsoft.com/azure/virtual-machines/windows/infrastructure-availability-sets-guidelines) (opravy a upgrady spravovat přes Azure) |
> | **Prostředek** |Vyhrazený  |Sdílet s ostatními klienty|
> | **Oblasti** |Datová centra |[Dvojice oblast](https://docs.microsoft.com/azure/virtual-machines/windows/regions-and-availability)|
> | **Úložiště** |Síť SAN nebo fyzické disky |[Spravovat Azure storage](https://azure.microsoft.com/pricing/details/managed-disks/?v=17.23h)|
> | **Škálování** |Vertikální Škálováním |Horizontální škálování|


### <a name="requirements"></a>Požadavky

- Určete rychlost velikosti a nárůst databáze hello.
- Určete požadavky hello IOPS, které můžete odhadnout na základě Oracle AWR sestavy nebo jiné sítě, nástroje pro sledování.

## <a name="configuration-options"></a>Možnosti konfigurace

Existují čtyři potenciálních oblastí, abyste mohli vyladit výkon tooimprove v prostředí Azure:

- Velikost virtuálního počítače
- Propustnost sítě
- Typy disků a konfigurace
- Nastavení mezipaměti na disku

### <a name="generate-an-awr-report"></a>Generování sestavy AWR

Pokud máte existující databázi Oracle a plánování toomigrate tooAzure, máte několik možností. Můžete spustit hello Oracle AWR sestavy tooget hello metriky (IOPS, MB/s, GiBs a tak dále). Zvolte hello virtuálních počítačů v závislosti na hello metriky, které jste shromáždili. Nebo se obrátit na váš tým tooget podobné informace o infrastruktuře.

Můžete zvážit spouštění sestavy AWR během pravidelné a špičkovým úlohy, takže můžete porovnat. Na základě těchto zpráv, může Velikost hello virtuální počítače založené na hello průměrné zatížení nebo hello maximální zátěž.

Tady je příklad toho, jak toogenerate zprávu o AWR:

```bash
$ sqlplus / as sysdba
SQL> EXEC DBMS_WORKLOAD_REPOSITORY.CREATE_SNAPSHOT;
SQL> @?/rdbms/admin/awrrpt.sql
```

### <a name="key-metrics"></a>Klíčové metriky

Následují hello metriky, které můžete získat z hello AWR sestavy:

- Celkový počet jader
- Procesor o rychlosti
- Celkové paměti v GB
- Využití procesoru
- Rychlost přenosu dat ve špičce
- Počet vstupně-výstupních operací změny (čtení a zápis)
- Vrátit protokolu rychlost (MB/s)
- Propustnost sítě
- Míra latence sítě (dolní nebo horní)
- Velikost databáze v GB
- Bajtů přijatých prostřednictvím SQL * Net z / tooclient

### <a name="virtual-machine-size"></a>Velikost virtuálního počítače

#### <a name="1-estimate-vm-size-based-on-cpu-memory-and-io-usage-from-hello-awr-report"></a>1. Odhad velikosti virtuálního počítače podle využití procesoru, paměti a vstupu a výstupu z hello AWR sestavy

Jednou z věcí, které může vypadat v je hello nejvyšší pět vypršel popředí události, které určují, kde se kritická místa systému hello.

V následujícím diagramu hello, například synchronizace souboru protokolu hello je v horní části hello. Označuje, hello počet počká, které jsou požadovány, než hello LGWR zapíše hello protokolu vyrovnávací paměti souboru protokolu toohello operaci znovu. Tyto výsledky naznačují, že jsou lépe provádění úložiště nebo disky. Hello diagram navíc také zobrazuje hello počet procesor (jádra) a hello množství paměti.

![Snímek obrazovky stránky sestavy AWR hello](./media/oracle-design/cpu_memory_info.png)

Hello následující diagram znázorňuje hello celkový počet vstupně-výstupní operace čtení a zápisu. Existovaly 59 GB číst a zapisovat během doby hello hello sestavy 247.3 GB.

![Snímek obrazovky stránky sestavy AWR hello](./media/oracle-design/io_info.png)

#### <a name="2-choose-a-vm"></a>2. Vyberte virtuální počítač

Na základě hello informací shromážděných z hello AWR sestavy, hello dalším krokem je toochoose virtuálních počítačů podobné velikosti, která vyhovuje vašim požadavkům. Seznam dostupných virtuálních počítačů najdete v článku hello [paměťově optimalizované](https://docs.microsoft.com/azure/virtual-machinFine tune es/virtual-machines-windows-sizes-memory).

#### <a name="3-fine-tune-hello-vm-sizing-with-a-similar-vm-series-based-on-hello-acu"></a>3. Upřesnit nastavení velikosti virtuálních počítačů hello podobné řady virtuálního počítače podle hello ACU

Poté, co jste vybrali hello virtuálních počítačů, věnujte pozornost toohello ACU pro hello virtuálních počítačů. Můžete vybrat jiný virtuální počítač podle hello ACU hodnotu, která lépe splňuje vaše požadavky. Další informace najdete v tématu [Azure výpočetní jednotky](https://docs.microsoft.com/azure/virtual-machines/windows/acu).

![Snímek obrazovky stránky jednotky ACU hello](./media/oracle-design/acu_units.png)

### <a name="network-throughput"></a>Propustnost sítě

Následující diagram ukazuje hello vztah mezi propustnost a IOPS Hello:

![Snímek obrazovky propustnost](./media/oracle-design/throughput.png)

propustnost sítě celkový Hello je odhadovaný podle hello následující informace:
- SQL * Net provoz
- MB/s x počet serverů (například Oracle Data Guard výstupní proud)
- Dalších faktorech, jako je například aplikace replikace

![Snímek obrazovky hello SQL * Net propustnost](./media/oracle-design/sqlnet_info.png)

Podle potřeb šířky pásma sítě, existují různé typy brány pro toochoose z. Mezi ně patří basic VpnGw a Azure ExpressRoute. Další informace najdete v tématu hello [brány VPN stránce s cenami](https://azure.microsoft.com/en-us/pricing/details/vpn-gateway/?v=17.23h).

**Recommendations** (Doporučení)

- Je latence sítě vyšší porovnání tooan místního nasazení. Snižuje sítě zaokrouhlí služebních cest může výrazně zlepšit výkon.
- tooreduce odezvy konsolidovat aplikace, které mají vysokou transakce nebo "chatty" aplikací na hello stejného virtuálního počítače.

### <a name="disk-types-and-configurations"></a>Typy disků a konfigurace

- *Výchozí OS disky*: tyto typy disků nabízejí trvalých dat a ukládání do mezipaměti. Jsou optimalizované pro přístup k operační systém při spuštění a není určeno pro buď transakcí nebo datového skladu (analytical) úlohy.

- *Nespravované disky*: pomocí těchto typů disku spravovat hello účty úložiště, které ukládají soubory hello virtuální pevný disk (VHD), které odpovídají tooyour disky virtuálních počítačů. Soubory VHD jsou uloženy jako objekty BLOB stránky v účtech úložiště Azure.

- *Spravované disky*: spravuje hello účty úložiště, které používáte pro disky virtuálních počítačů Azure. Určete typ disku hello (premium nebo standard) a velikost hello hello disku, které potřebujete. Azure vytváří a spravuje hello disku za vás.

- *Disky úložiště Premium*: tyto typy disků jsou nejvhodnější pro úlohy v produkčním prostředí. Premium storage podporuje VM disky, které může být připojen toospecific velikost series virtuálních počítačů, jako je například řady DS, DSv2, GS a F virtuálních počítačů. Hello premium disku se dodává s jinou velikostí, a můžete si vybrat mezi disky od too4 32 GB, 096 GB. Velikost každého disku má svou vlastní specifikace výkonu. V závislosti na požadavcích vaší aplikace můžete připojit jednoho nebo více disků tooyour virtuálních počítačů.

Když vytvoříte nový disk spravované z hello portálu, můžete hello **typ účtu** hello typu disku se má toouse. Mějte na paměti, že ne všechny dostupné disky jsou zobrazeny v rozevírací nabídce hello. Když vyberete konkrétní velikost virtuálního počítače, hello nabídce se zobrazí pouze hello úložiště k dispozici premium SKU, které jsou založeny na velikost tohoto virtuálního počítače.

![Snímek obrazovky stránky spravovaných disků na hello](./media/oracle-design/premium_disk01.png)

Další informace najdete v tématu [vysoce výkonné úložiště Premium a spravované disky pro virtuální počítače](https://docs.microsoft.com/azure/storage/storage-premium-storage).

Po dokončení konfigurace úložiště na virtuálním počítači, můžete chtít tooload testovací hello disků před vytvořením databáze. Protože víte, že hello vstupně-výstupních operací míry z hlediska latence a propustnosti vám pomohou určit, pokud virtuální počítače hello podporují hello očekává propustnost s latencí cíle.

Existuje několik nástrojů pro zatížení testování aplikací, jako je Oracle Orion, Sysbench a Fio.

Znovu spusťte hello zátěžový test, poté, co nasadíte databázi Oracle. Spuštění úlohy obyčejnými a špičkovým a hello zobrazit výsledky hello směrného plánu vašeho prostředí.

Může to být důležitější toosize hello úložiště založené na rychlost IOPS hello než velikost úložiště hello. Například v případě potřeby hello IOPS je 5 000, ale potřebujete jenom 200 GB může stále získáte hello P30 třída premium disku i když se dodává s více než 200 GB úložiště.

míra IOPS Hello je možné získat z hello AWR sestavy. Je dáno hello opakování protokolu, fyzických čtení a zápisů rychlost.

![Snímek obrazovky stránky sestavy AWR hello](./media/oracle-design/awr_report.png)

Například velikost znovu hello je 12,200,000 bajtů za sekundu, která je rovna too11.63 MB/s.
Hello IOPS je 12,200,000 / 2,358 = 5,174.

Až budete mít přehledné informace o hello vstupně-výstupní požadavky, můžete zvolit kombinaci jednotek, které jsou nejlépe hodí toomeet tyto požadavky.

**Recommendations** (Doporučení)

- Pro data tabulkového prostoru rozloženy hello vstupně-výstupní úlohy počet disků pomocí úložiště spravovaný nebo Oracle ASM.
- Jak pro operace náročné na čtení a zápisu náročných zvyšuje velikost bloku hello vstupně-výstupních operací, přidejte další datové disky.
- Zvětšete velikost bloku hello u velkých sekvenčních procesů.
- Používejte data komprese tooreduce vstupně-výstupní operace (pro data a indexů).
- Oddělené opakování protokoly, systém a temps a vrátit zpět TS na samostatné datové disky.
- Umístěte všechny soubory aplikace na výchozí disky operačního systému (/ dev/sda). Tyto disky nejsou optimalizovány pro rychlé virtuální počítač nemusí časy spuštění a poskytují dobrý výkon pro vaši aplikaci.

### <a name="disk-cache-settings"></a>Nastavení mezipaměti na disku

Existují tři možnosti pro ukládání do mezipaměti hostitele:

- *Jen pro čtení*: všechny požadavky jsou uložená v mezipaměti pro budoucí čtení. Jsou trvalé všech zápisů přímo tooAzure úložiště objektů Blob.

- *Čtení a zápis*: Toto je "čtení napřed" algoritmu. Hello čtení a zápisů jsou uložená v mezipaměti pro budoucí čtení. Provede zápis non zápisu prostřednictvím jsou nejprve trvalé toohello místní mezipaměti. Pro systém SQL Server jsou zápisy tooAzure trvalého úložiště, protože používá přímým zápisem. Také poskytuje nejnižší latenci disku hello pro lehké úlohy.

- *Žádný* (zakázáno): pomocí této možnosti můžete obejít hello mezipaměti. Všechna data hello je přenášená toodisk a trvalé tooAzure úložiště. Tato metoda poskytuje hello nejvyšší rychlost vstupně-výstupních operací, pro zatížení s intenzivním vstupně-výstupních operací. Budete také potřebovat tootake "transakce náklady" v úvahu.

**Recommendations** (Doporučení)

toomaximize hello propustnost, doporučujeme začínat **žádné** pro použití mezipaměti u hostitele. Pro Storage úrovně Premium, mějte překážek"hello" je nutné zakázat, když připojíte hello systém souborů s hello **jen pro čtení** nebo **žádné** možnosti. Aktualizujte soubor /etc/fstab hello s hello UUID toohello disky.

Další informace najdete v tématu [Premium úložiště pro virtuální počítače s Linuxem](https://docs.microsoft.com/azure/storage/storage-premium-storage#premium-storage-for-linux-vms).

![Snímek obrazovky stránky spravovaných disků na hello](./media/oracle-design/premium_disk02.png)

- Pro disky operačního systému, použít výchozí **pro čtení a zápis** ukládání do mezipaměti.
- Pro systém, TEMP a vrácení zpět pomocí **žádné** pro ukládání do mezipaměti.
- Pro DATA, použijte **žádné** pro ukládání do mezipaměti. Ale pokud vaše databáze je jen pro čtení nebo náročné na čtení, použijte **jen pro čtení** ukládání do mezipaměti.

Po uložení nastavení disku vaše data nelze změnit nastavení mezipaměti hostitele hello, pokud odpojit hello jednotku v hello úroveň operačního systému a znovu ji připojte po provedení hello změnit.


## <a name="security"></a>Zabezpečení

Po nastavení a konfiguraci prostředí Azure, dalším krokem hello je toosecure vaší sítě. Tady jsou některá doporučení:

- *Zásady skupiny NSG*: NSG může být definovaná podsíť nebo síťový adaptér. Je jednodušší toocontrol přístupu na úrovni podsítě hello pro zabezpečení a vynutit směrování pro věcmi, jako jsou brány firewall pro aplikaci.

- *Jumpbox*: lépe zabezpečeného přístupu správci nesmí připojovat přímo toohello aplikační služby nebo databáze. Jumpbox slouží jako média mezi hello správce počítače a prostředky Azure.
![Snímek obrazovky stránky topologie Jumpbox hello](./media/oracle-design/jumpbox.png)

    Hello správce počítače by měl nabízejí IP omezený přístup toohello pouze jumpbox. Hello jumpbox by měl mít přístup toohello aplikace a databáze.

- *Privátní sítě* (podsítě): doporučujeme mít hello aplikace služby a databáze v samostatných podsítích v tak lepší řízení lze nastavit pomocí zásad skupiny NSG.


## <a name="additional-reading"></a>Další čtení

- [Konfigurace Oracle ASM](configure-oracle-asm.md)
- [Konfigurace Oracle Data Guard](configure-oracle-dataguard.md)
- [Konfigurace brány Golden Oracle](configure-oracle-golden-gate.md)
- [Oracle zálohování a obnovení](oracle-backup-recovery.md)

## <a name="next-steps"></a>Další kroky

- [Kurz: Vytvoření vysoce dostupné virtuální počítače](../../linux/create-cli-complete.md)
- [Prozkoumejte ukázky rozhraní příkazového řádku Azure nasazení virtuálních počítačů](../../linux/cli-samples.md)
