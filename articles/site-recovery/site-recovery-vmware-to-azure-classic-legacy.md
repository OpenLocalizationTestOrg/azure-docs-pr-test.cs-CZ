---
title: "aaaReplicate virtuální počítače VMware a fyzické servery tooAzure (classic starší verze) | Microsoft Docs"
description: "Popisuje, jak tooreplicate místní virtuální počítače a tooAzure fyzických serverů Windows nebo Linuxem pomocí Azure Site Recovery ve starší verzi nasazení portálu classic hello."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: jwhit
editor: 
ms.assetid: f980fb52-8ec2-4dd9-85e2-8e52e449f1ba
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: raynew
ms.openlocfilehash: 64c6d906d54426fdd2b884350542a4562bb12528
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="replicate-vmware-virtual-machines-and-physical-servers-tooazure-with-azure-site-recovery-using-hello-classic-portal-legacy"></a>Replikovat virtuální počítače VMware a fyzické servery tooAzure s Azure Site Recovery pomocí portálu classic hello (zastaralé)
> [!div class="op_single_selector"]
> * [Azure Portal](site-recovery-vmware-to-azure.md)
> * [Portál Classic](site-recovery-vmware-to-azure-classic.md)
> * [Portál Classic (zastaralé)](site-recovery-vmware-to-azure-classic-legacy.md)
>
>

Vítejte tooAzure Site Recovery! Tento článek popisuje starší verze nasazení pro replikaci na lokální virtuální počítače VMware nebo tooAzure fyzických serverů Windows nebo Linuxem pomocí Azure Site Recovery na portálu classic hello.

## <a name="overview"></a>Přehled
Organizace potřebují strategii BCDR, která určuje, jak aplikace, úlohy a data zůstanou spuštěné a dostupné během plánovaných a neplánovaných výpadků a co nejdříve obnovit toonormal pracovní podmínky. Strategie BCDR by měla zajistit bezpečnost a obnovitelnost firemních dat a zajistit, aby v případě, že dojde k havárii, byly zpracovávané úlohy stále k dispozici.

Site Recovery je služba Azure, která přispívá tooyour strategie BCDR tím, že orchestruje replikaci místní fyzických serverů a virtuálních počítačů toohello cloudu (Azure) nebo tooa sekundárního datacentra. Pokud dojde k výpadkům ve vašem primárním umístění, můžete převzít toohello sekundárního umístění tookeep aplikace a úlohy, které jsou k dispozici. Primární umístění back tooyour nezdaří, až se obnoví toonormal operace. Další informace najdete v článku [Co je Azure Site Recovery](site-recovery-overview.md).

> [!WARNING]
> Tento článek obsahuje **starší verze pokyny**. Nepoužívejte ho pro nová nasazení. Místo toho [postupujte podle těchto pokynů](site-recovery-vmware-to-azure.md) toodeploy Site Recovery v hello portál Azure, nebo [použijte tyto pokyny](site-recovery-vmware-to-azure-classic.md) tooconfigure hello rozšířené nasazení portálu classic hello. Pokud jste už nasazená pomocí hello metody popsané v tomto článku, doporučujeme migraci toohello rozšířeného nasazení portálu classic hello.
>
>

## <a name="migrate-toohello-enhanced-deployment"></a>Migraci toohello rozšířeného nasazení
Tato část platí pouze pokud jste už nasazená Site Recovery pomocí hello pokyny v tomto článku.

toomigrate existující nasazení budete muset:

1. Nasaďte nové součásti Site Recovery ve vaší místní lokalitě.
2. Nakonfigurujte pověření pro automatické zjišťování virtuálních počítačů VMware na hello nové konfigurace serveru.
3. Vyhledat servery VMware hello s hello nové konfigurační server.
4. Vytvoří novou skupinu ochrany hello nové konfigurace serveru.

Než začnete, potřebujete:

* Doporučujeme, abyste nastavili údržby pro migraci.
* Hello **migraci počítačů** možnost je dostupná pouze v případě, že máte existující skupiny ochrany, které byly vytvořeny při nasazení starší verze.
* Po dokončení kroků migrace hello může trvat 15 minut nebo déle toorefresh hello pověření a toodiscover a obnovení virtuálních počítačů tak, aby je můžete přidat tooa skupiny ochrany. Můžete je aktualizovat ručně místo čekání.

Migraci následujícím způsobem:

1. Přečtěte si informace o [rozšířené nasazení portálu classic hello](site-recovery-vmware-to-azure-classic.md). Zkontrolujte hello rozšířené [architektura](site-recovery-vmware-to-azure-classic.md), a [požadavky](site-recovery-vmware-to-azure-classic.md).
2. Odinstalujte službu Mobility hello z počítače, které aktuálně replikujete. Nová verze služby hello bude nainstalována na hello počítače, když přidáte novou skupinu ochrany toohello.
3. Stáhněte si [registrační klíč trezoru](site-recovery-vmware-to-azure-classic.md) a [spusťte Průvodce instalací jednotná hello](site-recovery-vmware-to-azure-classic.md) tooinstall hello konfigurační server, procesový server a hlavní cíl součásti serveru. Další informace o [plánování kapacity](site-recovery-vmware-to-azure-classic.md).
4. [Nastavit přihlašovací údaje](site-recovery-vmware-to-azure-classic.md) obnovení lokality můžete použít tooaccess VMware server tooautomatically zjišťování virtuálních počítačů VMware. Další informace o [požadovaná oprávnění](site-recovery-vmware-to-azure-classic.md).
5. Přidat [vCenter servery nebo hostitelů vSphere](site-recovery-vmware-to-azure-classic.md). Může trvat 15 minut, další informace pro servery tooappear na portálu Site Recovery hello.
6. Vytvoření [nové skupiny ochrany](site-recovery-vmware-to-azure-classic.md). Může to trvat až minut too15 hello portálu toorefresh tak, aby hello virtuální počítače jsou zjišťovány a zobrazí. Pokud nechcete, aby toowait můžete zvýraznit název serveru pro správu hello (nemáte klikněte na něj) > **aktualizovat**.
7. V části hello nové skupiny ochrany klikněte na **migraci počítačů**.

    ![Přidat účet](./media/site-recovery-vmware-to-azure-classic-legacy/legacy-migration1.png)
8. V **vyberte počítače** skupiny ochrany vyberte hello chcete toomigrate z a počítačů, které chcete toomigrate hello.

    ![Přidat účet](./media/site-recovery-vmware-to-azure-classic-legacy/legacy-migration2.png)
9. V **nakonfigurovat nastavení cílového** určit, zda má toouse hello stejné nastavení pro všechny počítače a vyberte hello procesový server a účet úložiště Azure. Pokud nemáte samostatný procesový server bude jím hello hello IP adresa serveru hello konfigurace serveru.

    ![Přidat účet](./media/site-recovery-vmware-to-azure-classic-legacy/legacy-migration3.png)

    > [AZURE.NOTE] [Migrace účtů úložiště](../resource-group-move-resources.md) napříč prostředků skupiny v hello stejného předplatného, nebo ve předplatných není podporována pro účty úložiště používá pro nasazení Site Recovery.

1. V **zadejte účty**, vyberte účet hello jste vytvořili pro hello proces serveru tooaccess hello počítač toopush hello nová verze služby Mobility hello.

   ![Přidat účet](./media/site-recovery-vmware-to-azure-classic-legacy/legacy-migration4.png)
2. Site Recovery bude migrovat váš účet úložiště Azure toohello replikovaná data, která jste zadali. Volitelně můžete znovu použít hello účtu úložiště, který jste použili v nasazení hello starší verze.
3. Po hello úlohy budou automaticky synchronizovat dokončení virtuálních počítačů. Po dokončení synchronizace můžete odstranit ze skupiny ochrany starší verze hello hello virtuálních počítačů.
4. Po migraci všech počítačů můžete odstranit hello starší verze chráněnou skupinu.
5. Mějte na paměti, toospecify hello převzetí služeb při selhání vlastnosti pro počítače, a po dokončení synchronizace hello nastavení sítě Azure.
6. Pokud máte existující plány obnovení, migrací toohello rozšířeného nasazení s hello **migrovat plán obnovení** možnost. Měli byste jenom to provést po migraci všechny chráněné počítače.

   ![Přidat účet](./media/site-recovery-vmware-to-azure-classic-legacy/legacy-migration5.png)

> [!NOTE]
> Po dokončení migrace pokračovat hello [rozšířené článku](site-recovery-vmware-to-azure-classic.md). Hello zbývající části tohoto článku starší verze bude relevantní a nepotřebujete toofollow všechny více hello kroků popsaných v it **.
>
>

## <a name="what-do-i-need"></a>Co musím udělat?
Tento diagram zobrazuje součásti nasazení hello.

![Nový trezor](./media/site-recovery-vmware-to-azure-classic-legacy/architecture.png)

Zde je seznam toho, co budete potřebovat:

| **Komponenta** | **Nasazení** | **Podrobnosti** |
| --- | --- | --- |
| **Konfigurační server** |Standardní A3 virtuální počítač Azure v hello stejnému předplatnému jako Site Recovery. |konfigurační server Hello koordinuje komunikaci mezi chráněné počítače, hello procesový server a hlavních cílových serverů v Azure. Když dojde k převzetí služeb při selhání, nastaví se replikace a obnovení souřadnice v Azure. |
| **Hlavní cílový server** |Virtuální počítač Azure – serveru systému Windows na základě v galerii bitovou kopii systému Windows Server 2012 R2 (tooprotect Windows počítače) nebo jako Linux server podle Galerie bitovou kopii OpenLogic CentOS 6.6 (počítače se systémem Linux tooprotect).<br/><br/> Tři nastavení velikosti možností, které jsou k dispozici – standardní A4, standardní D14 a standardní DS4.<br/><br/> Hello je server připojený toohello stejné síti Azure jako hello konfigurační server.<br/><br/> Nastavení na portálu Site Recovery hello |Přijetí a uchovává replikovaná data z vaší chráněných počítačů pomocí připojených virtuálních pevných disků na úložiště objektů blob v účtu úložiště Azure vytvořit.<br/><br/> Vyberte standardní DS4 speciálně pro konfiguraci ochrany pro úlohy vyžadující konzistentní vysoký výkon a nízkou latencí pomocí prémiový účet úložiště. |
| **Procesový server** |Místní virtuální nebo fyzická serveru se systémem Windows Server 2012 R2<br/><br/> Doporučujeme vám, že se má umístit na hello stejné síti a segment sítě LAN jako hello počítače, že chcete tooprotect, ale můžete spustit v jiné síti, dokud chráněného počítače mají L3 sítě tooit viditelnosti.<br/><br/> Ho nastavit a zaregistrovat ji na portálu Site Recovery hello toohello konfigurační server. |Chráněné počítače odeslat replikace dat toohello místně procesový server. Obsahuje data replikace toocache diskové mezipaměti, kterou přijme. Provede několik akcí na tato data.<br/><br/> Optimalizuje data ukládání do mezipaměti, komprese a šifrování před odesláním na toohello hlavní cílový server.<br/><br/> Zpracovává nabízená instalace služby Mobility hello.<br/><br/> Provádí automatického zjišťování virtuálních počítačů VMware. |
| **Místní počítače** |Místní virtuální počítače VMware nebo fyzické servery se systémem Windows nebo Linux. |Nakonfigurujete nastavení replikace, které se vztahují na jeden nebo více počítačů. Přes jednotlivé počítače nebo běžně, může selhat více počítačů, které shromažďování do plánu obnovení. |
| **Služba mobility** |Nainstalovat na každý virtuální počítač nebo na fyzický server chcete tooprotect<br/><br/> Můžete nainstalovat ručně nebo nabídnutých a automaticky nainstaluje server hello proces při povolíte replikaci pro počítač. |Hello služba Mobility odešle data toohello procesový server během počáteční replikace (synchronizaci). Po hello počítač je v chráněném stavu (po dokončení nové synchronizace) hello služba Mobility zaznamená zápisy toodisk v paměti a odešle je toohello procesový server. Applicationconsistency pro systémy Windows Server je dosaženo pomocí služby VSS. |
| **Trezor služby Azure Site Recovery** |Vytvoření trezoru Site Recovery předplatné a zaregistrujte server v trezoru hello. |Trezor Hello koordinuje a orchestruje replikaci, převzetí služeb při selhání a obnovení mezi místními servery a Azure. |
| **Mechanismus replikace** |**Přes hello Internet**– komunikuje a replikují data z chráněných místní servery tooAzure pomocí přes zabezpečený kanál SSH/TLS hello Internetu. Toto je výchozí možnost hello.<br/><br/> **Sítě VPN nebo ExpressRoute**– komunikuje a replikují data mezi místními servery a Azure prostřednictvím připojení VPN. Budete potřebovat tooset site-to-site VPN nebo spojení ExpressRoute mezi místními servery a sítě Azure.<br/><br/> Vyberte způsob tooreplicate během nasazování Site Recovery. Mechanismus hello nelze změnit po dokončení konfigurace bez dopadu na replikaci existující počítačů. |Žádná možnost vyžaduje tooopen můžete všechny příchozí síťové porty na chráněných počítačích. Veškerá komunikace sítě je inicializována z místní lokality hello. |

## <a name="capacity-planning"></a>Plánování kapacity
budete potřebovat tooconsider hlavní oblasti Hello:

* **Zdrojové prostředí**– hello infrastruktury VMware, nastavení zdrojového počítače a požadavky.
* **Serverech součástí**– hello procesový server, konfigurační server a hlavní cílový server

### <a name="considerations-for-hello-source-environment"></a>Důležité informace týkající se hello zdrojové prostředí
* **Maximální velikost disku**– hello aktuální maximální velikost hello disk, který může být připojené tooa virtuálního počítače je 1 TB. Maximální velikost zdrojového disku, který je možné replikovat hello je proto také omezené too1 TB.
* **Maximální velikost na zdroj**– hello maximální velikost jeden zdrojový počítač je 31 TB (s disky, 31) a s instancí D14 zřízené pro hello hlavní cílový server.
* **Počet zdrojů na hlavním cílovém serveru**– více zdrojového počítače se dají chránit pomocí jednoho hlavní cílový server. Ale jeden zdrojový počítač nelze chránit napříč více hlavních cílových serverů, protože jako replikovat disky, virtuální pevný disk, který odpovídá hello velikost disku hello je vytvořen v úložišti objektů blob v Azure a připojené jako datový disk toohello hlavní cílový server.  
* **Maximální denní míry změn na zdroj**– existují tři faktory, které je třeba toobe považována za při zvažování hello doporučená změna četnosti podle zdroje. Důležité informace na základě cílové hello na hello cílový disk pro každou operaci na zdroji hello se vyžadují dva IOPS. Je to proto, že čtení stará data a zápis nová data hello proběhne na hello cílový disk.
  * **Každý den změnit rychlost podporovanou hello procesový server**– zdrojový počítač nemůžou zahrnovat víc serverů procesu. Jeden proces serveru může podporovat až too1 TB denní míry změn. Proto je 1 TB hello maximální denní data změnit rychlost pro zdrojový počítač nepodporuje.
  * **Maximální propustnost nepodporuje hello cílový disk**– maximální změn na zdrojový disk nemůže být více než 144 GB a den (s 8 kb velikost zápisu). Najdete hello tabulce v části hlavní cíl hello hello propustnost a IOPs hello cíle pro různé velikosti zápisu. Toto číslo musí být rozdělen ve dvou, protože každý zdroj IOP generuje 2 IOPS na hello cílový disk. Přečtěte si informace o [Azure škálovatelnosti a cílech výkonnosti](../storage/common/storage-scalability-targets.md#scalability-targets-for-virtual-machine-disks) při konfiguraci hello cíl pro účty úložiště premium.
  * **Maximální propustnost nepodporuje účet úložiště hello**– zdroj nemůžou zahrnovat víc účtů úložiště. Vzhledem k že trvá účtu úložiště nesmí být delší než 20 000 požadavků za sekundu a že každý zdroj IOP generuje 2 IOPS na hello hlavní cílový server, doporučujeme, abyste že zachovat hello počet IOPS napříč hello zdroj too10, 000. Přečtěte si informace o [Azure škálovatelnosti a cílech výkonnosti](../storage/common/storage-scalability-targets.md#scalability-targets-for-virtual-machine-disks) při konfiguraci hello zdroj pro účty úložiště premium.

### <a name="considerations-for-component-servers"></a>Důležité informace týkající se serverech součástí
Tabulka 1 shrnuje hello velikostí virtuálních počítačů pro konfiguraci hello a hlavních cílových serverů.

| **Komponenta** | **Nasazené instancemi Azure** | **Počet jader** | **Paměť** | **Maximální počet disků** | **Velikost disku** |
| --- | --- | --- | --- | --- | --- |
| Konfigurace serveru |Standardní A3 |4 |7 GB |8 |1023 GB |
| Hlavní cílový server |Standardní A4 |8 |14 GB |16 |1023 GB |
| Standardní D14 |16 |112 GB |32 |1023 GB | |
| Standardní DS4 |8 |28 GB |16 |1023 GB | |

**Tabulka 1**

#### <a name="process-server-considerations"></a>Důležité informace o zpracování serveru
Obecně proces serveru velikosti závisí na denní míry změny hello mezi všechny chráněné úlohy.

* Budete potřebovat dostatek výpočetního tooperform úloh vložené komprese a šifrování.
* procesový server Hello používá mezipaměti založené na disku. Ujistěte se, zda text hello doporučená místa v mezipaměti a disk propustnost je k dispozici toofacilitate hello data změny uložené v události hello přetížení sítě nebo výpadek.
* Zajistěte dostatečnou šířku pásma, tak, aby hello procesový server můžete nahrát hello data toohello hlavní cílový server tooprovide trvalou ochranu dat.

Tabulka 2 poskytuje souhrn hello proces serveru pokyny.

| **Míry změny dat** | **VYUŽITÍ PROCESORU** | **Paměť** | **Velikost disku mezipaměti** | **Propustnost disku mezipaměti** | **Vstupní/výstupní šířky pásma** |
| --- | --- | --- | --- | --- | --- |
| < 300 GB |4 Vcpu (2 sockets * @ 2.5 GHz 2 jádra) |4 GB |600 GB |7 too10 MB za sekundu |30 MB/s nebo 21 MB/s |
| 300 too600 GB |8 Vcpu (2 sockets * @ 2.5 GHz 4 jádra) |6 GB |600 GB |11 too15 MB za sekundu |60 MB/s nebo 42 MB/s |
| 600 GB too1 TB |12 Vcpu (2 sockets * @ 2.5 GHz 6 jader) |8 GB |600 GB |16 too20 MB za sekundu |100 MB/s nebo 70 MB/s |
| > 1 TB |Nasazení jiný procesový server | | | | |

**Tabulka 2**

Kde:

* Příjem příchozích dat je stažení šířky pásma (intranetu mezi hello proces a zdrojového serveru).
* Odchozí je nahrávání šířky pásma (internet mezi hello procesový server a hlavní cílový server). Odchozí čísla předpokládá průměrná komprese 30 % procesu serveru.
* Pro mezipaměť samostatný disk operačního systému minimální 128 GB disk se doporučuje pro všechny servery procesu.
* Pro hello propustnost disku mezipaměti následující úložiště byla použita pro srovnávací testy: 8 jednotky SAS 10 ot. / s konfiguraci RAID 10.

#### <a name="configuration-server-considerations"></a>Požadavky na konfiguraci serveru
Každý konfigurační server může podporovat až too100 zdrojový počítač se svazky 3 – 4. Pokud vaše nasazení je větší, doporučujeme že nasadit další konfigurační server. Vlastnosti virtuálního počítače výchozí hello hello konfigurace serveru najdete v tabulce 1.

#### <a name="master-target-server-and-storage-account-considerations"></a>Hlavní cílový server a úložiště důležité informace o účtu
Hello úložiště pro každou hlavní cílový server obsahuje disk s operačním systémem, svazek pro uchovávání dat a datových disků. jednotka pro uchování Hello udržuje hello deník změny na disku pro hello trvání intervalem pro uchovávání dat hello definované v portálu Site Recovery hello.  Odkazovat tooTable 1 pro vlastnosti virtuálního počítače hello hello hlavní cílový server. Tabulka 3 ukazuje, jak se používají hello disky A4.

| **Instance** | **Disk s operačním systémem** | **Uchování** | **Datové disky** |
| --- | --- | --- | --- |
| **Uchování** |**Datové disky** | | |
| Standardní A4 |disk 1 (1 * 1023 GB) |disk 1 (1 * 1023 GB) |15 disky (15 * 1023 GB) |
| Standardní D14 |disk 1 (1 * 1023 GB) |disk 1 (1 * 1023 GB) |31 disky (15 * 1023 GB) |
| Standardní DS4 |disk 1 (1 * 1023 GB) |disk 1 (1 * 1023 GB) |15 disky (15 * 1023 GB) |

**Tabulka 3**

Plánování kapacity pro hlavní cílový server hello závisí na:

* Výkon úložiště Azure a omezení
  * maximální počet k Hello vysoce využívat disky pro standardní vrstvy virtuálního počítače, je o 40 (20 000/500 IOPS na disk) v jednom úložném účtu. Přečtěte si informace o [cíle škálovatelnosti pro účty úložiště standard storage](../storage/common/storage-scalability-targets.md#scalability-targets-for-virtual-machine-disks) a [prémiové účty úložiště](../storage/common/storage-scalability-targets.md#scalability-targets-for-virtual-machine-disks).
* Denní míry změn
* Uchování svazku úložiště.

Poznámky:

* Jeden zdroj nemůžou zahrnovat víc účtů úložiště. To platí toohello datový disk, který přejít účty úložiště toohello vybrali při konfiguraci ochrany. disky Hello operačního systému a hello uchování obvykle přejděte toohello automaticky nasazeny účet úložiště.
* svazek úložiště pro uchovávání dat Hello požadované závisí na denní míry změny hello a hello počet dní uchovávání informací. uchování úložiště požadované za hlavní cílový server Hello = celkového objemu změn ze zdroje za den * počet dní uchovávání informací.
* Každý hlavní cílový server obsahuje pouze jeden svazek pro uchovávání. svazek pro uchovávání dat Hello byl sdílen napříč hello disky připojené toohello hlavní cílový server. Například:
  * Pokud je zdrojový počítač s 5 disků a každý disk generuje 120 (8 kb velikost) IOPS na zdroji hello, to znamená, že too240 IOPS na disk (na disku cílového hello na zdroj vstupně-výstupních operací 2 operací). 240 IOPS je v rámci hello Azure za maximální IOPS disku 500.
  * Na hello svazek pro uchovávání dat, to všechno bude 120 * 5 = 600 IOPS a to se může stát zúžené bottle. V tomto scénáři by strategii je dobré se tooadd další svazek pro uchovávání dat toohello disky a span ji napříč, jako stripe konfigurace RAID. Tím se zvyšuje výkon, protože hello IOPS jsou rozmístěny v několika jednotkách. Hello počet jednotek toobe přidat svazek pro uchovávání dat toohello bude takto:
    * Celkový počet IOPS ze zdrojové prostředí / 500
    * Celkového objemu změn za den (nekomprimovaným) na zdrojové prostředí nebo 287 GB. 287 GB je maximální propustnost hello nepodporuje cílový disk za den. Tato metrika budou lišit v závislosti na velikosti zápisu hello s faktor 8 kB, protože v takovém případě je 8 kb thí předpokládá, že velikost zápisu. Pokud velikost zápis hello je 4K, propustnost bude 287/2. A pokud je velikost zápisu hello 16 kB pak propustnost bude 287 * 2.
* počet účtů úložiště vyžaduje Hello = source celkový počet IOPs/10000.

## <a name="before-you-start"></a>Než začnete
| **Komponenta** | **Požadavky** | **Podrobnosti** |
| --- | --- | --- |
| **Účet Azure** |Budete potřebovat účet [Microsoft Azure](https://azure.microsoft.com/). Můžete začít s [bezplatnou zkušební verzí](https://azure.microsoft.com/pricing/free-trial/). | |
| **Úložiště Azure** |Budete potřebovat data toostore replikovat účtu úložiště Azure<br/><br/> Buď hello účet by měl být [standardní geograficky redundantní účet úložiště](../storage/common/storage-redundancy.md#geo-redundant-storage) nebo [prémiový účet úložiště](../storage/common/storage-premium-storage.md).<br/><br/> Musí v hello stejné oblasti jako hello služba Azure Site Recovery a být přidružen k hello stejného předplatného. Nepodporujeme hello přesun účty úložiště vytvořené pomocí hello [nový portál Azure](../storage/common/storage-create-storage-account.md) mezi skupinami prostředků.<br/><br/> toolearn více číst [Úvod tooMicrosoft Azure Storage](../storage/common/storage-introduction.md) | |
| **Virtuální síť Azure** |Budete potřebovat virtuální síť Azure na které hello se nasadí konfigurační server a hlavní cílový server. Musí být v hello stejném předplatném, oblasti jako trezor Azure Site Recovery hello. Pokud chcete tooreplicate data přes ExpressRoute nebo VPN připojení hello Azure virtuální síť musí být připojené tooyour do místní sítě prostřednictvím připojení ExpressRoute nebo VPN typu Site-to-Site. | |
| **Prostředky Azure** |Ujistěte se, že máte dostatek prostředků Azure toodeploy všechny součásti. Další informace najdete v [limity předplatného Azure](../azure-subscription-service-limits.md). | |
| **Virtuální počítače Azure** |Virtuální počítače, které chcete tooprotect musí být v souladu s [požadavky Azure](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements).<br/><br/> **Disk počet**– na jednom chráněného serveru může podporovat maximálně 31 disků<br/><br/> **Velikost disku**– kapacita jednotlivých disků nesmí být větší než 1 023 GB<br/><br/> **Clustering**– servery clusteru nejsou podporovány.<br/><br/> **Spouštěcí**– Firmware rozhraní UEFI (Unified Extensible) / není podporované spouštění Extensible Firmware Interface<br/><br/> **Svazky**– Bitlocker šifrované svazky nejsou podporované.<br/><br/> **Názvy serverů**– názvy musí obsahovat 1 až 63 znaků (písmena, číslice a pomlčky). Hello název musí začínat písmenem nebo číslicí a končit písmenem nebo číslicí. Po počítač je chráněný můžete upravit hello název Azure. | |
| **Konfigurační server** |Standardní virtuální počítač A3 založena na imagi Galerie Azure Site Recovery Windows Server 2012 R2 se vytvoří ve vašem předplatném pro hello konfigurační server. Je vytvořen jako hello první instance v novou cloudovou službu. Pokud vyberete veřejného Internetu jako hello typ připojení pro hello konfigurace serveru hello cloudové služby se vytvoří pomocí vyhrazené veřejné IP adresy.<br/><br/> cesta instalace Hello by měly být pouze anglické znaky. | |
| **Hlavní cílový server** |Virtuální počítač Azure, standardní A4, D14 nebo DS4.<br/><br/> cesta instalace Hello by měly být pouze anglické znaky. Například hello cesta by měla být **/usr/local/ASR** u hlavního cílového serveru se systémem Linux. | |
| **Procesový server** |Můžete nasadit procesový server hello na fyzický nebo virtuální počítač se systémem Windows Server 2012 R2 s nejnovějšími aktualizacemi hello. Nainstalujte na jednotce C: /.<br/><br/> Doporučujeme umístit hello server na hello stejnou síť a podsíť jako hello počítače chcete tooprotect.<br/><br/> Nainstalujte VMware rozhraní příkazového řádku vSphere 5.5.0 na hello procesový server. na serveru proces hello v pořadí toodiscover virtuálních počítačů spravovaných pomocí systému vCenter server nebo virtuální počítače běžící na hostiteli ESXi je vyžadován Hello VMware vSphere rozhraní příkazového řádku součásti.<br/><br/> cesta instalace Hello by měly být pouze anglické znaky.<br/><br/> Systém souborů reFS nepodporuje. | |
| **VMware** |Správa vašeho hypervisory VMware vSphere server VMware vCenter. Měla by být spuštěna s vCenter verze 5.1 nebo 5.5 s nejnovějšími aktualizacemi hello.<br/><br/> Jeden nebo více hypervisory vSphere obsahující virtuální počítače VMware, budete chtít, aby tooprotect. Hello hypervisor by měl běžet ESX/ESXi verze 5.1 nebo 5.5 s nejnovějšími aktualizacemi hello.<br/><br/> Virtuální počítače VMware by měl mít nástroje VMware nainstalovaná a spuštěná. | |
| **Počítače s Windows** |Chráněné fyzických serverech nebo virtuálních počítačů VMware s Windows mají několik požadavků.<br/><br/> A podporovaný 64bitový operační systém: **Windows Server 2012 R2**, **systému Windows Server 2012**, nebo **Windows Server 2008 R2 s v minimálně SP1**.<br/><br/> Hello název hostitele, přípojné body, zařízení názvů, cestu v systému Windows (např: C:\Windows) by měly být pouze v angličtině.<br/><br/> Hello operačního systému by měly být nainstalovány na jednotce C:\.<br/><br/> Jsou podporovány pouze základní disky. Dynamické disky nejsou podporovány.<br/><br/> Pravidla brány firewall na chráněné počítače by mohly tooreach hello konfigurace a hlavním cílovým serverům v Azure.p ><p>Budete potřebovat tooprovide účtu správce (musí být místní správce na počítači Windows hello) toopush nainstalovat na serverech Windows hello služba Mobility. Pokud je účet hello zadaný účet nepřipojená k doméně musíte toodisable řízení vzdáleného přístupu uživatele v místním počítači hello. toodo tento hello přidat položku registru LocalAccountTokenFilterPolicy DWORD s hodnotou 1 v části HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System. položky registru hello tooadd z rozhraní příkazového řádku otevřete cmd nebo prostředí powershell a zadejte  **`REG ADD HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System /v LocalAccountTokenFilterPolicy /t REG_DWORD /d 1`** . [Další informace](https://msdn.microsoft.com/library/aa826699.aspx) o řízení přístupu.<br/><br/> Po převzetí služeb při selhání Pokud chcete připojení tooWindows virtuálních počítačů v Azure pomocí vzdálené plochy Ujistěte se, zda je povoleno vzdálené plochy hello na místním počítači. Pokud se nepřipojujete prostřednictvím sítě VPN, pravidla brány firewall musí povolit připojení ke vzdálené ploše přes hello Internetu. | |
| **Počítače se systémem Linux** |Podporovaný 64bitový operační systém: **Centos 6.4, 6.5, 6.6**; **Oracle Enterprise Linux 6.4, 6.5 systémem hello Red Hat kompatibilní jádra nebo nedělitelné Enterprise jádra verze 3 (UEK3)**, **SUSE Linux Enterprise Server 11 SP3**.<br/><br/> Pravidla brány firewall na chráněné počítače by mohly tooreach hello konfigurace a hlavních cílových serverů v Azure.<br/><br/> soubory/etc/hosts na chráněných počítačích by měly obsahovat položky, které mapují hello místního hostitele název tooIP adres přidružených všechny síťové adaptéry <br/><br/> Pokud chcete, aby tooconnect tooan Azure virtuální počítače spuštěné Linux po převzetí služeb při selhání pomocí klienta Secure Shell (ssh), ujistěte se, že služba Secure Shell na hello chráněné hello toostart automaticky při spuštění systému, a počítači, pravidla brány firewall umožňují ssh tooit připojení.<br/><br/> název hostitele Hello, přípojné body, zařízení a Linux systémové cesty a názvy souborů (např/etc /; USR) by měla být v angličtině jenom.<br/><br/> Můžete povolit ochranu pro místní počítače hello následující úložiště:-<br>Systém souborů: EXT3, ETX4, ReiserFS, XFS<br>Funkce Multipath softwaru zařízení Mapper (multipath)<br>Správce svazků: LVM2<br>Fyzické servery s HP CCISS řadič úložiště nejsou podporovány. | |
| **Třetí strany** |Některé součásti nasazení v tomto scénáři závisí na software jiných výrobců toofunction správně. Úplný seznam najdete v části [oznámení softwaru třetích stran a informace](#third-party) | |

### <a name="network-connectivity"></a>Připojení k síti
Máte dvě možnosti při konfiguraci připojení k síti mezi místními servery a hello Azure virtuální sítě, na které hello se nasadí infrastrukturu součásti (konfigurační server, hlavních cílových serverů). Budete potřebovat toodecide které síťové připojení možnost toouse předtím, než můžete nasadit konfigurační server. Toto nastavení v době hello nasazení budete potřebovat toochoose. Nelze změnit později.

**Internet:** komunikace a replikace dat mezi místními servery hello (procesový server, chráněných počítačů) a serverech součástí infrastruktury Azure hello (konfigurační server, hlavní cílový server) se stane přes zabezpečený protokol SSL nebo Připojení TLS z místní toohello veřejné koncové body na serverech hello konfigurace a hlavní cíl. (hello jedinou výjimkou je hello připojení mezi hello procesový server a hlavní cílový server hello na portu TCP 9080, což nešifrované. Jenom řízení informace týkající se, že se vyměňují toohello protokol replikace pro nastavení replikace na tomto připojení.)

![Diagram nasazení Internetu](./media/site-recovery-vmware-to-azure-classic-legacy/internet-deployment.png)

**VPN**: komunikace a replikace dat mezi místními servery hello (procesový server, chráněných počítačů) a serverech součástí infrastruktury Azure hello (konfigurační server, hlavní cílový server) se stane prostřednictvím připojení VPN mezi místní sítí a hello Azure virtuální sítě, na které hello nasazených konfigurační server a hlavních cílových serverů. Zajistěte, aby byl v místní síti připojené toohello virtuální síť Azure připojení ExpressRoute nebo připojení site-to-site VPN.

![Diagram nasazení sítě VPN](./media/site-recovery-vmware-to-azure-classic-legacy/vpn-deployment.png)

## <a name="step-1-create-a-vault"></a>Krok 1: Vytvoření trezoru
1. Přihlaste se toohello [portálu pro správu](https://portal.azure.com).
2. Rozbalte položku **datové služby** > **služeb zotavení** a klikněte na tlačítko **trezor Site Recovery**.
3. Klikněte na **Vytvořit nový** > **Rychlé vytvoření**.
4. V **název**, zadejte popisný název tooidentify hello trezoru.
5. V **oblast**, hello vyberte zeměpisnou oblast trezoru hello. toocheck podporované oblasti, najdete v části geografickou dostupností v [Azure Site Recovery podrobnosti o cenách](https://azure.microsoft.com/pricing/details/site-recovery/)
6. Klikněte na **Vytvořit trezor**.

    ![Nový trezor](./media/site-recovery-vmware-to-azure-classic-legacy/quick-start-create-vault.png)

Zkontrolujte, zda tooconfirm hello stav panelu, který hello trezor byl úspěšně vytvořen. Hello trezor bude uvedený jako **Active** na hello hlavní **služeb zotavení** stránky.

## <a name="step-2-deploy-a-configuration-server"></a>Krok 2: Nasazení konfigurace serveru
### <a name="configure-server-settings"></a>Nakonfigurujte nastavení serveru
1. V hello **služeb zotavení** klikněte na tlačítko hello trezoru tooopen hello stránky rychlý Start. Rychlý Start můžete otevřít také kdykoli pomocí ikony hello.

    ![Ikona Rychlý start](./media/site-recovery-vmware-to-azure-classic-legacy/quick-start-icon.png)
2. V rozevíracím seznamu hello vyberte **mezi místní lokalitě VMware nebo fyzické servery a Azure**.
3. V **Příprava prostředků Target(Azure)** klikněte na tlačítko **nasazení konfigurační Server**.

    ![Nasazení konfigurace serveru](./media/site-recovery-vmware-to-azure-classic-legacy/deploy-cs2.png)
4. V **nové podrobnosti o konfiguraci serveru** zadejte:

   * Název hello konfigurace serveru a přihlašovací údaje tooconnect tooit.
   * V typu připojení k síti hello rozevírací nabídky vyberte **veřejného Internetu** nebo **VPN**. Všimněte si, že nelze změnit toto nastavení po jeho použití.
   * Vyberte hello síť Azure, na které hello server by měl být umístěn. Pokud používáte VPN zkontrolujte zda hello síť Azure je připojený tooyour do místní sítě, podle očekávání.
   * Zadejte hello interní IP adresu a podsíť, která bude přiřazena toohello serveru. Všimněte si, že hello první čtyři IP adresy v žádné podsíti jsou vyhrazené pro interní použití Azure. Použijte všechny další dostupnou IP adresu.

     ![Nasazení konfigurace serveru](./media/site-recovery-vmware-to-azure-classic-legacy/cs-details.png)
5. Když kliknete na tlačítko **OK** standardní virtuální počítač A3 založena na imagi Galerie Azure Site Recovery Windows Server 2012 R2 se vytvoří ve vašem předplatném pro hello konfigurační server. Je vytvořen jako hello první instance v novou cloudovou službu. Pokud jste vybrali tooconnect přes hello internet hello cloudové služby se vytvoří pomocí vyhrazené veřejné IP adresy. Můžete sledovat průběh v hello **úlohy** kartě.

    ![Monitorování průběhu](./media/site-recovery-vmware-to-azure-classic-legacy/monitor-cs.png)
6. Pokud se připojujete přes internet, hello po hello konfigurační server je nasazený Poznámka hello veřejné IP tooit adresu přiřazenou na hello **virtuální počítače** stránku hello portálu Azure. Pak na hello **koncové body** kartě Poznámka hello veřejný port HTTPS namapované tooprivate port 443. Tyto informace později budete potřebovat při registraci hlavního cíle hello a proces servery s hello konfigurační server. konfigurační server Hello se nasazuje s těmito koncovými body:

   * HTTPS: Veřejný port je použité toocoordinate komunikace mezi servery součásti a hello Azure přes internet. Privátní port 443 je použité toocoordinate komunikaci mezi servery součásti a Azure prostřednictvím sítě VPN.
   * Vlastní: Veřejný port se používá pro navrácení služeb po obnovení nástroj komunikaci přes hello Internetu. Privátní port 9443 se používá pro navrácení služeb po obnovení nástroj komunikaci prostřednictvím sítě VPN.
   * Prostředí PowerShell: Privátní port 5986
   * Vzdálená plocha: privátní port 3389

   ![Koncové body virtuálních počítačů](./media/site-recovery-vmware-to-azure-classic-legacy/vm-endpoints.png)

   > [!WARNING]
   > Nepřidávejte odstranit, nebo změňte číslo portu veřejných nebo privátních hello žádné koncové body vytvořené během nasazení konfiguračního serveru.
   >
   >

Hello konfigurační server je nasazena v automaticky vytvořené cloudové služby Azure s vyhrazenou IP adresu. Hello rezervovanou adresu je potřebné tooensure, který hello konfigurace serveru cloudové služby IP adresu zůstane stejný hello po restartování počítače hello virtuálních počítačů (včetně hello konfigurační server) na hello cloudové služby. Hello vyhrazené veřejné IP adresy potřebovat toobe ručně nerezervované při hello konfigurační server je vyřazena z provozu nebo budete zůstanou vyhrazené. Výchozí limit 20 vyhrazené veřejné IP adresy každé předplatné není k dispozici. [Další informace](../virtual-network/virtual-networks-reserved-private-ip.md) o vyhrazené IP adresy.

### <a name="register-hello-configuration-server-in-hello-vault"></a>Zaregistrujte konfigurační server hello v trezoru hello
1. V hello **rychlý Start** klikněte na stránce **Příprava cílové prostředky** > **stáhněte si registrační klíč**. soubor klíče Hello je generován automaticky. Je platný po dobu 5 dní, po jeho vygenerování. Zkopírujte jej toohello konfigurační server.
2. V **virtuální počítače** vyberte hello konfigurační server hello seznamu virtuálních počítačů. Otevřete hello **řídicí panel** a klikněte na **Connect**. **Otevřete** hello stáhnout toolog soubor RDP na konfigurační server hello pomocí vzdálené plochy. Pokud používáte sítě VPN, použijte pro připojení ke vzdálené ploše z místní lokality hello hello interní IP adresu (hello adresu, kterou jste zadali při nasazení hello konfigurační server). Hello instalace Průvodce konfigurace serveru Azure Site Recovery se spustí automaticky při přihlášení pro hello poprvé.

    ![Registrace](./media/site-recovery-vmware-to-azure-classic-legacy/splash.png)
3. V **instalace softwaru třetích stran** klikněte na tlačítko **souhlasím** toodownload a nainstalujte MySQL.

    ![Instalace MySQL](./media/site-recovery-vmware-to-azure-classic-legacy/sql-eula.png)
4. V **podrobnosti o serveru databáze MySQL** vytvořit přihlašovací údaje toolog na instanci serveru MySQL hello.

    ![Přihlašovací údaje databáze MySQL](./media/site-recovery-vmware-to-azure-classic-legacy/sql-password.png)
5. V **nastavení Internetu** zadejte, jak konfigurační server hello se budou připojovat toohello Internetu. Poznámky:

   * Pokud chcete, aby toouse vlastní proxy server můžete musí ho nastavit před instalací hello zprostředkovatele.
   * Když kliknete na tlačítko **Další** testu se spustí toocheck hello proxy připojení.
   * Pokud používáte vlastní proxy server nebo váš výchozí proxy server vyžaduje ověření, budete potřebovat tooenter hello podrobnosti o proxy serveru, včetně hello adresu, port a přihlašovací údaje.
   * Hello následující adresy URL by měla být přístupné přes proxy server hello:
     * *.hypervrecoverymanager.windowsazure.com
     * *.accesscontrol.windows.net
     * *.backup.windowsazure.com
     * *.blob.core.windows.net
     * *.store.core.windows.net
   * Pokud máte IP adresy na základě pravidel brány firewall Ověřte, zda hello pravidla jsou nastaveny tooallow komunikace z hello konfigurace serveru toohello IP adresy popsané v [rozsahy IP Datacentra Azure](https://msdn.microsoft.com/library/azure/dn175718.aspx) a protokol HTTPS (443). By měla mít toowhite seznam rozsahů IP hello oblast Azure, abyste naplánovali toouse a západní USA.

     ![Registrace proxy serveru](./media/site-recovery-vmware-to-azure-classic-legacy/register-proxy.png)
6. V **nastavení zprostředkovatele chybových zpráv lokalizace** zadat jazyk, který chcete tooappear chybové zprávy.

    ![Chybová zpráva registrace](./media/site-recovery-vmware-to-azure-classic-legacy/register-locale.png)
7. V **Azure Site Recovery registrace** Procházet a vyberte hello soubor klíče jste zkopírovali toohello serveru.

    ![Soubor klíče registrace](./media/site-recovery-vmware-to-azure-classic-legacy/register-vault.png)
8. Na stránce dokončení hello hello průvodce vyberte tyto možnosti:

   * Vyberte **spustit dialogové okno správy účtu** toospecify, který hello dialogové okno Správa účtů by měla otevřít po dokončení Průvodce hello.
   * Vyberte **vytvoření ikony na ploše pro Cspsconfigtool** tooadd zástupce na ploše na hello konfigurační server, aby mohli otevřít hello **Správa účtů** dialogové okno kdykoli bez nutnosti toorerun hello Průvodce.

     ![Dokončení registrace](./media/site-recovery-vmware-to-azure-classic-legacy/register-final.png)
9. Klikněte na tlačítko **Dokončit** toocomplete hello průvodce. Generuje se přístupové heslo. Zkopírujte jej tooa zabezpečeného umístění. Budete ho potřebovat tooauthenticate a zaregistrujte hello proces a hlavních cílových serverů s hello konfigurační server. Také použila integrity tooensure kanál v komunikaci se serverem konfigurace. Můžete obnovit heslo hello ale pak budete potřebovat toore registrace hello hlavní cíl a proces servery pomocí hello nové heslo.

    ![Přístupové heslo](./media/site-recovery-vmware-to-azure-classic-legacy/passphrase.png)

Po registraci bude hello konfigurační server uvedený na hello **konfigurační servery** stránky v trezoru hello.

### <a name="set-up-and-manage-accounts"></a>Nastavit a spravovat účty
Během nasazování Site Recovery požadavky přihlašovací údaje pro hello následující akce:

* Účet VMware, takže thatSite obnovení můžete automaticky zjišťování virtuálních počítačů na server vCenter nebo hostitelů vSphere.
* Když přidáte počítačů pro ochranu, tak, aby Site Recovery můžete nainstalovat službu Mobility hello na ně.

Poté, co jste registrováni hello konfigurační server můžete otevřít hello **Správa účtů** tooadd dialogové okno a Správa účtů, které se mají použít pro tyto akce. Existuje několik způsobů toodo toto:

* Otevřete hello zástupce jste se rozhodli toocreated pro dialogové okno hello na poslední stránku hello instalačního programu pro konfigurační server hello (cspsconfigtool).
* Dialogové okno otevřete hello na dokončení konfigurace nastavení serveru.

1. V **Správa účtů** klikněte na tlačítko **přidat účet**. Můžete také upravit a odstranit existující účty.

    ![Správa účtů](./media/site-recovery-vmware-to-azure-classic-legacy/manage-account.png)
2. V **Podrobnosti účtu** zadejte název toouse účtu ve službě Azure a přihlašovací údaje (název domény a uživatelské).

    ![Správa účtů](./media/site-recovery-vmware-to-azure-classic-legacy/account-details.png)

### <a name="connect-toohello-configuration-server"></a>Připojení toohello konfigurace serveru
Existují dva způsoby tooconnect toohello konfigurační server:

* Přes VPN typu site-to-site nebo připojením ExpressRoute
* Přes hello internet

Poznámky:

* Připojení k Internetu používá ve spojení s hello veřejná virtuální IP adresa serveru hello koncové body hello hello virtuálního počítače.
* Připojení VPN používá hello interní IP adresu serveru hello společně s privátní porty hello koncových bodů.
* Jestli je jednorázové rozhodnutí toodecide tooconnect (řízení a replikace dat) z vaší místní servery toohello různé servery součásti (konfigurační server, hlavní cílový server) běžící v Azure prostřednictvím připojení VPN nebo hello Internetu. Nelze změnit, tato nastavení později. V takovém případě je budete potřebovat tooredeploy hello scénář a znovu aktivujte ochranu vašeho počítače.  

## <a name="step-3-deploy-hello-master-target-server"></a>Krok 3: Nasaďte hello hlavní cílový server
1. Klikněte na tlačítko **Příprava prostředků Target(Azure)** > **nasadit hlavní cílový server**.
2. Zadejte podrobnosti o hello hlavního cílového serveru a přihlašovací údaje. Hello server nasazen v hello stejné síti Azure jako hello konfigurační server. Po kliknutí na tlačítko toocomplete vytvoří virtuální počítač Azure s Galerie bitovou kopii systému Windows nebo Linux.

    ![Nastavení cílového serveru](./media/site-recovery-vmware-to-azure-classic-legacy/target-details.png)

Všimněte si, že hello první čtyři IP adresy v žádné podsíti jsou vyhrazené pro interní použití Azure. Zadejte jakékoli další dostupnou IP adresu.

> [!NOTE]
> Při konfiguraci ochrany pro úlohy, které vyžadují konzistentní vysoké vstupně-výstupní výkon a nízkou latencí v pořadí toohost vstupně-výstupních operací zatížení s intenzivním pomocí vyberte standardní DS4 [prémiový účet úložiště](../storage/common/storage-premium-storage.md).
>
>

1. Windows hlavní cílový server, který virtuální počítač je vytvořen s těmito koncovými body. Všimněte si, že veřejné koncové body jsou vytvořeny pouze v případě, že vaše připojující se přes internet hello.

   * Vlastní: Veřejný port je používán hello proces serveru toosend replikace dat přes hello Internetu. Privátní port 9443 používají hello proces serveru toosend replikace dat toohello hlavní cílový server prostřednictvím sítě VPN.
   * Vlastní 1: Veřejný port je používán hello proces serveru toosend metadata přes hello Internetu. Privátní port 9080 používají hello proces serveru toosend metadata toohello hlavní cílový server prostřednictvím sítě VPN.
   * Prostředí PowerShell: Privátní port 5986
   * Vzdálená plocha: privátní port 3389
2. Hlavní cílový server Linux virtuálního počítače je vytvořen s těmito koncovými body. Všimněte si, že veřejné koncové body jsou vytvořeny pouze v případě, že se připojujete přes hello Internetu.

   * Vlastní: Veřejný port je používán proces serveru toosend replikace dat přes hello Internetu. Privátní port 9443 používají hello proces serveru toosend replikace dat toohello hlavní cílový server prostřednictvím sítě VPN.
   * Vlastní 1: Veřejný port je používán hello proces serveru toosend metadata přes hello Internetu. Privátní port 9080 používá hello proces serveru toosend metadata toohello hlavní cílový server prostřednictvím sítě VPN
   * SSH: Privátní port 22

     > [!WARNING]
     > Nepřidávejte odstranit, nebo změňte číslo portu veřejných nebo privátních hello každého hello koncové body vytvořené během nasazení hello hlavního cílového serveru.
     >
     >
3. V **virtuální počítače** počkejte toostart hello virtuálního počítače.

   * Pokud je Windows server Poznámka dolů hello vzdálené plochy podrobnosti.
   * Pokud je Linux server a se připojujete přes VPN Poznámka hello interní IP adresu hello virtuálního počítače. Pokud se připojujete přes hello internet Poznámka hello veřejnou IP adresu.
4. Přihlaste se instalace toocomplete hello serveru a zaregistrovat ji pomocí hello konfigurační server.
5. Pokud používáte systém Windows:

   1. Inicializujte připojení ke vzdálené ploše toohello virtuálního počítače. Hello při prvním přihlášení skript se spustí v okně prostředí PowerShell. Nemáte, zavřete ji. Po dokončení nástroj Konfigurace hostitele agenta hello automaticky otevře tooregister hello serveru.
   2. V **konfigurace agenta hostitele** zadejte hello interní IP adresu hello konfigurace serveru a port 443. Můžete použít interní adresa hello a privátní port 443, i když nejsou připojení prostřednictvím sítě VPN, protože hello virtuální počítač je připojený toohello stejné síti Azure jako hello konfigurační server. Nechte **použití HTTPS** povolena. Zadejte přístupové heslo hello hello konfigurační server, který jste si předtím poznamenali. Klikněte na tlačítko **OK** tooregister hello serveru. Hello NAT možnosti, můžete ignorovat. Nejsou používají.
   3. Pokud vaše odhadovaná uchování požadavek jednotky je více než 1 TB můžete nakonfigurovat hello svazek pro uchovávání dat (R:) pomocí virtuální disk a [prostory úložiště](http://blogs.technet.com/b/askpfeplat/archive/2013/10/21/storage-spaces-how-to-configure-storage-tiers-with-windows-server-2012-r2.aspx)

   ![Hlavní cílový server Windows](./media/site-recovery-vmware-to-azure-classic-legacy/target-register.png)
6. Pokud používáte systém Linux:

   1. Ujistěte se, že jste nainstalovali hello nejnovější Linux integrační služby (LIS) nainstalována před instalací hello hlavní cílový server. Můžete najít hello nejnovější verzi služby LIS společně s pokyny o tom, tooinstall [zde](https://www.microsoft.com/download/details.aspx?id=46842). Po instalaci hello LIS, restartujte počítač hello.
   2. V **prostředky Příprava cíle (Azure)** klikněte na tlačítko **stáhnout a nainstalovat další software (jenom pro Linux hlavní cílový Server)**. Kopírování hello stáhnout vkládání souboru toohello virtuálního počítače pomocí protokolu sftp klienta. Případně můžete přihlásit toohello nasazené linux hlavní cílový server a používat *wget http://go.microsoft.com/fwlink/?LinkID=529757&clcid=0x409* toodownload hello hello souboru.
   3. Přihlaste se na serveru toohello pomocí klienta Secure Shell. Pokud jste připojení toohello síť Azure prostřednictvím sítě VPN pomocí hello interní IP adresu. Jinak použijte hello externí IP adresu a veřejný koncový bod SSH hello.
   4. Extrahujte soubory hello z instalačního programu hello algoritmem gzip spuštěním: **funkce vkládání – xvzf Microsoft-ASR_UA_8.4.0.0_RHEL6-64***
      ![Linux hlavní cílový server](./media/site-recovery-vmware-to-azure-classic-legacy/linux-tar.png)
   5. Ujistěte se, můžete začít hello directory toowhich jste extrahovali obsah souboru vkládání hello hello.
   6. Kopírovat hello konfigurace serveru přístupové heslo tooa místní soubor pomocí příkazu hello **echo  *`<passphrase>`*  > passphrase.txt**
   7. Spusťte příkaz hello "**sudo. / install -t obou - a -R hostitele /usr/local/ASR MasterTarget -d -i  *`<Configuration server internal IP address>`*  -p 443 -s y - c https -P passphrase.txt**".

      ![Registrovat cílový server](./media/site-recovery-vmware-to-azure-classic-legacy/linux-mt-install.png)
7. Počkejte několik minut (10 až 15) a na zkontrolujte stránku hello této hello hlavní cílový server označen jako zaregistrovaný v **servery** > **konfigurační servery** **podrobnosti o serveru** kartě. Pokud používáte systém Linux a nezaregistroval hello hostitele konfigurační nástroj spusťte znovu z /usr/local/ASR/Vx/bin/hostconfigcli. Oprávnění k přístupu tooset budete potřebovat spuštěním chmod jako kořenového adresáře.

    ![Zkontrolujte cílový server](./media/site-recovery-vmware-to-azure-classic-legacy/target-server-list.png)

> [!NOTE]
> To může trvat až too15 minut po dokončení pro hello hlavní cílový server toobe uvedené hello portálu registrace. okamžitě, klikněte na tlačítko tooupdate **aktualizovat** na hello **konfigurační servery** stránky.
>
>

## <a name="step-4-deploy-hello-on-premises-process-server"></a>Krok 4: Nasazení hello místně procesový server
Před zahájením doporučujeme nakonfigurovat statickou IP adresu na hello procesový server tak, aby zajistil, že toobe trvalé napříč restartuje.

1. Klikněte na tlačítko rychlý Start > **místní instalaci Server proces** > **stáhnout a nainstalovat procesní server hello**.

    ![Instalace serveru procesu](./media/site-recovery-vmware-to-azure-classic-legacy/ps-deploy.png)
2. Kopírování hello stáhnout zip souboru toohello serveru, na kterém budete tooinstall hello procesový server. soubor zip Hello obsahuje dvě instalačních souborů:

   * Microsoft-ASR_CX_TP_8.4.0.0_Windows*
   * Microsoft-ASR_CX_8.4.0.0_Windows*
3. Rozbalte hello archivu a zkopírujte hello soubory tooa umístění instalace na hello server.
4. Spustit hello **Microsoft-ASR_CX_TP_8.4.0.0_Windows*** instalační soubor a postupujte podle pokynů hello. Tím se nainstaluje komponenty jiných výrobců, které jsou potřebné pro nasazení hello.
5. Spusťte **Microsoft-ASR_CX_8.4.0.0_Windows***.
6. Na hello **režim serveru** vyberte **procesový Server**.
7. Na hello **prostředí podrobnosti** stránku hello následující:

    - Pokud chcete, klikněte na virtuální počítače VMware tooprotect **Ano**
    - Pokud chcete pouze tooprotect fyzických serverů a proto není nutné nainstalovat na server hello proces vCLI VMware. Klikněte na tlačítko **ne** a pokračovat.

1. Všimněte si následujících hello při instalaci VMware vCLI:

   * **Je podporován pouze VMware rozhraní příkazového řádku vSphere 5.5.0**. procesový server Hello nefunguje s jinými verzemi nebo aktualizace rozhraní příkazového řádku vSphere.
   * Stáhnout 5.5.0 rozhraní příkazového řádku vSphere z [sem.](https://my.vmware.com/web/vmware/details?downloadGroup=VCLI550&productId=352)
   * Pokud jste nainstalovali rozhraní příkazového řádku vSphere těsně před jste spustili instalaci hello procesový server a instalační program nezjišťuje, počkejte na toofive minut, než se znovu spusťte instalaci. Tím se zajistí, že všechny proměnné prostředí hello potřebné pro zjišťování rozhraní příkazového řádku vSphere mít správně inicializován.
2. V **výběr síťovou kartu pro procesový Server** hello vyberte síťový adaptér měli používat tento server hello procesu.

   ![Vyberte adaptér](./media/site-recovery-vmware-to-azure-classic-legacy/ps-nic.png)
3. V **podrobnosti o konfiguraci serveru**:

   * Pro IP adresu hello zadejte adresu a port, pokud se připojujete prostřednictvím sítě VPN hello interní IP adresu hello konfigurační server a 443 pro hello port. V opačném případě zadejte hello veřejná virtuální IP adresy a namapované veřejný koncový bod HTTP.
   * Zadejte přístupové heslo hello hello konfigurace serveru.
   * Zrušte **ověřte Mobility service softwaru podpis** Pokud chcete toodisable ověření při použití automatické nabízené tooinstall hello služby. Ověření podpisu vyžaduje připojení k Internetu z hello procesový server.
   * Klikněte na **Další**.

   ![Zaregistrujte konfigurační server](./media/site-recovery-vmware-to-azure-classic-legacy/ps-cs.png)
4. V **vyberte instalační jednotce** vyberte jednotky mezipaměti. procesový server Hello musí mezipaměti jednotku s alespoň 600 GB volného místa. Pak klikněte na **Nainstalovat**.

   ![Zaregistrujte konfigurační server](./media/site-recovery-vmware-to-azure-classic-legacy/ps-cache.png)
5. Všimněte si, že může být nutné toorestart hello serveru toocomplete hello instalace. V **konfigurační Server** > **podrobnosti o serveru** zkontrolujte, že procesový server hello se zobrazí a je úspěšně zaregistrovaný v trezoru hello.

> [!NOTE]
> To může trvat až too15 minut po dokončení pro hello proces serveru tooappear najdete v části hello konfigurační server registrace. konfigurační server hello tooupdate okamžitě, aktualizujte kliknutím na tlačítko Aktualizovat hello dole hello stránku hello konfigurace serveru
>
>

![Ověření procesového serveru](./media/site-recovery-vmware-to-azure-classic-legacy/ps-register.png)

Pokud nebylo zakážete ověření podpisu pro službu Mobility hello při registraci hello procesového serveru můžete provést později následujícím způsobem:

1. Přihlaste se hello procesový server jako správce a otevřete soubor hello C:\pushinstallsvc\pushinstaller.conf pro úpravy. Části hello **[PushInstaller.transport]** přidejte tento řádek: **SignatureVerificationChecks = "0"**. Uložte a zavřete soubor hello.
2. Restartujte hello InMage PushInstall služby.

## <a name="step-5-update-site-recovery-components"></a>Krok 5: Aktualizace Site Recovery součásti
Součásti Site Recovery jsou aktualizovány ze tootime čas. Když jsou k dispozici nové aktualizace byste měli instalovat v hello následující pořadí:

1. Konfigurace serveru
2. Procesový server
3. Hlavní cílový server
4. Nástroj pro navrácení služeb po obnovení (vContinuum)

### <a name="obtain-and-install-hello-updates"></a>Získat a nainstalovat aktualizace hello
1. Aktualizace pro konfiguraci hello, proces a hlavních cílových serverů můžete získat z hello Site Recovery **řídicí panel**. Pro Linux instalace extrahujte soubory hello z instalačního programu hello algoritmem gzip a spusťte příkaz hello "sudo. / install" tooinstall hello aktualizace.
2. [Stáhněte si](http://go.microsoft.com/fwlink/?LinkID=533813) hello nejnovější aktualizace pro navrácení služeb po obnovení tool(vContinuum) hello.
3. Pokud používáte virtuální počítače nebo fyzické servery, které už máte službu Mobility hello nainstalován, můžete získat aktualizace pro službu hello následujícím způsobem:

   * **Možnost 1**: Stažení aktualizací:
     * [Windows Server (pouze 64bitová verze)](http://download.microsoft.com/download/8/4/8/8487F25A-E7D9-4810-99E4-6C18DF13A6D3/Microsoft-ASR_UA_8.4.0.0_Windows_GA_28Jul2015_release.exe)
     * [CentOS 6.4,6.5,6.6 (pouze 64bitová verze)](http://download.microsoft.com/download/7/E/D/7ED50614-1FE1-41F8-B4D2-25D73F623E9B/Microsoft-ASR_UA_8.4.0.0_RHEL6-64_GA_28Jul2015_release.tar.gz)
     * [Oracle 6.4,6.5 Enterprise Linux (pouze 64bitová verze)](http://download.microsoft.com/download/5/2/6/526AFE4B-7280-4DC6-B10B-BA3FD18B8091/Microsoft-ASR_UA_8.4.0.0_OL6-64_GA_28Jul2015_release.tar.gz)
     * [SUSE Linux Enterprise Server SP3 (pouze 64bitová verze)](http://download.microsoft.com/download/B/4/2/B4229162-C25C-4DB2-AD40-D0AE90F92305/Microsoft-ASR_UA_8.4.0.0_SLES11-SP3-64_GA_28Jul2015_release.tar.gz)
     * Po aktualizaci hello proces server hello bude aktualizovanou verzi služby Mobility hello k dispozici ve složce C:\pushinstallsvc\repository hello na hello procesový server.
   * **Možnost 2**: Pokud máte počítač s starší verze hello nainstalovaná služba Mobility, dokáže automaticky upgradovat na počítači hello službu Mobility hello z portálu pro správu hello.

     1. Zkontrolujte, zda že tento proces server hello se aktualizuje.
     2. Ujistěte se, že hello chráněný počítač je v souladu s hello [požadavky](#install-the-mobility-service-automatically) pro automatické vkládání hello služby Mobility, tak, aby aktualizace hello funguje podle očekávání.
     3. Vyberte hello skupiny ochrany, zvýraznění hello chráněný počítač a klikněte na tlačítko **služba Mobility aktualizace**. Toto tlačítko je k dispozici, pokud existuje novější verze služby Mobility hello pouze.

         ![Vyberte vCenter server](./media/site-recovery-vmware-to-azure-classic-legacy/update-mobility.png)

V účtech vyberte zadejte hello správce účtu toobe používá tooupdate hello služba mobility na server hello chráněné. Klikněte na tlačítko OK a počkejte toocomplete hello aktivuje úlohu.

## <a name="step-6-add-vcenter-servers-or-vsphere-hosts"></a>Krok 6: Přidejte servery vCenter nebo hostitelů vSphere
1. Klikněte na tlačítko **servery** > **konfigurační servery** > konfigurační server >**přidání systému vCenter Server** tooadd vCenter server nebo vSphere hostitele.

    ![Vyberte vCenter server](./media/site-recovery-vmware-to-azure-classic-legacy/add-vcenter.png)
2. Zadejte podrobnosti pro hello server nebo hostitele a vyberte hello procesového serveru, které budou použité toodiscover ho.

   * Pokud hello vCenter server není spuštěn na portu 443 výchozí hello zadejte číslo portu hello, na které hello je spuštěný vCenter server.
   * Hello procesový server musí být na hello stejné sítě jako hostitelů server vSphere vCenter hello a měl by obsahovat VMware vSphere 5.5.0 nainstalované rozhraní příkazového řádku.

     ![nastavení serveru vCenter](./media/site-recovery-vmware-to-azure-classic-legacy/add-vcenter4.png)
3. Po dokončení zjišťování serveru vCenter hello se objeví v části Podrobnosti o serveru configuration hello.

    ![nastavení serveru vCenter](./media/site-recovery-vmware-to-azure-classic-legacy/add-vcenter2.png)
4. Pokud používáte server hello tooadd účet bez oprávnění správce nebo hostitele, ujistěte se, že má účet hello hello následující oprávnění:

   * vCenter účty musí mít Datacenter, úložiště, složky, hostitele, sítě, prostředků, úložiště zobrazení, virtuální počítač a vSphere distribuované přepínač oprávnění povoleno.
   * účty vSphere hostitele by měl mít hello Datacenter, úložiště, složky, hostitele, sítě, prostředků, virtuální počítač a vSphere distribuované přepínač oprávnění povoleno

## <a name="step-7-create-a-protection-group"></a>Krok 7: Vytvoření skupiny ochrany
1. Otevřete **chráněné položky** > **skupiny ochrany** > **vytvořit skupinu ochrany**.

    ![Vytvořit skupinu ochrany](./media/site-recovery-vmware-to-azure-classic-legacy/create-pg1.png)
2. Na hello **zadejte nastavení skupiny ochrany** stránky zadejte název pro skupinu hello a vyberte hello konfigurační server, na kterém chcete toocreate hello skupiny.

    ![Nastavení skupiny ochrany](./media/site-recovery-vmware-to-azure-classic-legacy/create-pg2.png)
3. Na hello **zadejte nastavení replikace** stránku konfigurace hello nastavení replikace, která se použije pro všechny počítače hello ve skupině hello.

    ![Skupiny ochrany replikace](./media/site-recovery-vmware-to-azure-classic-legacy/create-pg3.png)
4. Nastavení:

   * **Konzistence pro víc Virtuálních**: Pokud tuto možnost zapnout se vytváří body obnovení sdílené konzistentní s aplikací mezi hello počítačů ve skupině ochrany hello. Toto nastavení je nejdůležitější, když všechny počítače hello ve skupině ochrany hello běží stejné zatížení hello. Všechny počítače budou obnovené toohello stejného datového bodu. K dispozici je pouze pro servery systému Windows.
   * **Prahová hodnota RPO**: Pokud hello replikace průběžné ochrany dat RPO překročí prahovou hodnotu RPO hello nakonfigurované se budou generovat výstrahy.
   * **Uchování bodu obnovení**: Určuje interval pro uchovávání hello. Chráněné počítače může být obnovena tooany bodu v rámci tohoto okna.
   * **Frekvence snímkování konzistentní aplikace vzhledem**: Určuje, jak často budou vytvořeny body obnovení obsahující snímky konzistentní s aplikacemi.

Skupiny ochrany hello můžete sledovat, jak jste vytvořili na hello **chráněné položky** stránky.

## <a name="step-8-set-up-machines-you-want-tooprotect"></a>Krok 8: Nastavení počítače chcete tooprotect
Budete potřebovat tooinstall hello služba Mobility na virtuální počítače a fyzické servery, které chcete tooprotect. Můžete provést dvěma způsoby:

* Automaticky push a nainstalujte na každém počítači ze serveru proces hello hello služby.
* Ručně nainstalujte službu hello.

### <a name="install-hello-mobility-service-automatically"></a>Automaticky nainstalovat službu Mobility hello
Při přidání počítače tooa ochranu skupiny hello služba Mobility je automaticky instaluje a nainstalovat na každém počítači s hello procesový server.

**Automaticky instalace push na serverech Windows hello služby mobility:**

1. Nainstalujte nejnovější aktualizace hello pro hello procesového serveru, jak je popsáno v [krok 5: nainstalujte nejnovější aktualizace](#step-5-install-latest-updates)a ujistěte se, že procesový server hello je k dispozici.
2. Ujistěte se, je síťové připojení mezi hello zdrojového počítače a hello procesový server a že hello zdrojový počítač je přístupný z hello procesový server.  
3. Konfigurace tooallow brány firewall systému Windows hello **sdílení souborů a tiskáren** a **Windows Management Instrumentation**. V části nastavení brány Windows Firewall vyberte možnost hello "Povolit aplikaci nebo funkci průchod bránou Firewall" a vyberte aplikace hello, jak ukazuje následující obrázek hello. Pro počítače, které patří tooa domény hello zásady brány firewall můžete nakonfigurovat objekt zásad skupiny.

    ![Nastavení brány firewall](./media/site-recovery-vmware-to-azure-classic-legacy/push-firewall.png)
4. Hello účet používaný tooperform hello nabízenou instalaci musí být hello skupiny Administrators na počítači hello že chcete tooprotect. Tyto přihlašovací údaje se používají jenom pro nabízenou instalaci služby Mobility hello a poskytnete jim při přidání skupiny ochrany tooa počítače.
5. Pokud hello za předpokladu, že účet není účet domény budete potřebovat toodisable řízení vzdáleného přístupu uživatele v místním počítači hello. toodo tento hello přidat položku registru LocalAccountTokenFilterPolicy DWORD s hodnotou 1 v části HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System. položky registru hello tooadd z rozhraní příkazového řádku otevřete cmd nebo prostředí powershell a zadejte  **`REG ADD HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System /v LocalAccountTokenFilterPolicy /t REG_DWORD /d 1`** .

**Automaticky nabízená instalace služby mobility hello na servery se systémem Linux:**

1. Nainstalujte nejnovější aktualizace hello pro hello procesového serveru, jak je popsáno v [krok 5: nainstalujte nejnovější aktualizace](#step-5-install-latest-updates)a ujistěte se, že procesový server hello je k dispozici.
2. Ujistěte se, je síťové připojení mezi hello zdrojového počítače a hello procesový server a že hello zdrojový počítač je přístupný z hello procesový server.  
3. Zajistěte, aby byl hello účtu uživatele root na zdrojovém serveru se Linux hello.
4. Ujistěte se, že soubor/etc/hosts hello na zdroji hello Linux server obsahuje položky, které mapují hello místního hostitele název tooIP adres přidružených všechny síťové adaptéry.
5. Nainstalujte nejnovější openssh hello, openssh-server, balíčky openssl na počítači hello chcete tooprotect.
6. Ujistěte se, že je povolený protokol SSH, a že běží na portu 22.
7. Povolte ověřování pomocí protokolu SFTP subsystém a heslo v souboru sshd_config hello následujícím způsobem:

   * (a) Přihlaste se jako kořenový adresář.
   * b) v hello soubor/etc/ssh/sshd_config souboru najít hello řádek, který začíná **PasswordAuthentication**.
   * c) zrušte komentář u hello řádku a změňte hodnotu "Ne" hello příliš "Ano".

       ![Linux mobility](./media/site-recovery-vmware-to-azure-classic-legacy/linux-push.png)
   * d) řádek hello najít, který začíná subsystém a zrušte komentář u hello řádku.

       ![Linux nabízené mobility](./media/site-recovery-vmware-to-azure-classic-legacy/linux-push2.png)    
8. Zkontrolujte, zda je podporována hello zdrojový počítač Linux variant.

### <a name="install-hello-mobility-service-manually"></a>Ruční instalace služby Mobility hello
softwarové balíčky Hello používá tooinstall hello Mobility služby jsou na C:\pushinstallsvc\repository hello procesového serveru. Přihlášení do hello procesový server a kopírování hello příslušné instalační balíček toohello zdrojového počítače podle následující tabulky hello:-

| Zdrojový operační systém | Balíček služby mobility na procesovém serveru |
| --- | --- |
| Windows Server (pouze 64bitová verze) |`C:\pushinstallsvc\repository\Microsoft-ASR_UA_8.4.0.0_Windows_GA_28Jul2015_release.exe` |
| CentOS 6.4, 6.5, 6.6 (pouze 64bitové verze) |`C:\pushinstallsvc\repository\Microsoft-ASR_UA_8.4.0.0_RHEL6-64_GA_28Jul2015_release.tar.gz` |
| SUSE Linux Enterprise Server 11 SP3 (pouze 64bitová verze) |`C:\pushinstallsvc\repository\Microsoft-ASR_UA_8.4.0.0_SLES11-SP3-64_GA_28Jul2015_release.tar.gz` |
| Oracle Enterprise Linux 6.4, 6.5 (pouze 64bitová verze) |`C:\pushinstallsvc\repository\Microsoft-ASR_UA_8.4.0.0_OL6-64_GA_28Jul2015_release.tar.gz` |

**tooinstall hello na Windows serveru ručně službu Mobility**, hello následující:

1. Kopírování hello **Microsoft ASR_UA_8.4.0.0_Windows_GA_28Jul2015_release.exe** balíčku z cesty adresáře serveru proces hello uvedené v tabulce hello výše toohello zdrojového počítače.
2. Instalace služby Mobility hello spuštěním hello spustitelný soubor na zdrojovém počítači hello.
3. Postupujte podle pokynů instalačního programu hello.
4. Vyberte **služba Mobility** hello role a klikněte na **Další**.

    ![Instalaci služby mobility](./media/site-recovery-vmware-to-azure-classic-legacy/ms-install.png)
5. Nechte hello instalační adresář jako hello výchozí cestu instalace a klikněte na **nainstalovat**.
6. V **konfigurace agenta hostitele** určete hello IP adresu a port HTTPS hello konfigurace serveru.

   * Pokud se připojujete přes internet zadejte hello hello jako hello port veřejná virtuální IP adresy a veřejný koncový bod HTTPS.
   * Pokud se připojujete prostřednictvím sítě VPN zadejte hello interní IP adresu a 443 pro hello port. Nechte **použití HTTPS** zaškrtnutí.

     ![Instalaci služby mobility](./media/site-recovery-vmware-to-azure-classic-legacy/ms-install2.png)
7. Zadejte heslo hello konfiguračního serveru a klikněte na tlačítko **OK** tooregister hello služba Mobility s hello konfigurační server.

**toorun z hello příkazového řádku:**

1. Zkopírujte hello heslo ze hello CX toohello souboru "C:\connection.passphrase" na serveru hello a spusťte tento příkaz. V našem příkladu CX i 104.40.75.37 a hello HTTPS port je 62519:

    `C:\Microsoft-ASR_UA_8.2.0.0_Windows_PREVIEW_20Mar2015_Release.exe" -ip 104.40.75.37 -port 62519 -mode UA /LOG="C:\stdout.txt" /DIR="C:\Program Files (x86)\Microsoft Azure Site Recovery" /VERYSILENT  /SUPPRESSMSGBOXES /norestart  -usesysvolumes  /CommunicationMode https /PassphrasePath "C:\connection.passphrase"`

**Instalaci služby Mobility hello ručně na Linux server**:

1. Zkopírujte archivu odpovídající vkládání hello podle hello tabulce výše, od hello proces serveru toohello zdrojového počítače.
2. Otevřete program prostředí a extrahovat hello komprimované vkládání archivu tooa místní cestu spuštěním`tar -xvzf Microsoft-ASR_UA_8.2.0.0*`
3. Vytvořte soubor passphrase.txt do místního adresáře toowhich hello jste extrahovali obsah hello hello vkládání archivu zadáním  *`echo <passphrase> >passphrase.txt`*  z prostředí.
4. Instalaci služby Mobility hello zadáním  *`sudo ./install -t both -a host -R Agent -d /usr/local/ASR -i <IP address> -p <port> -s y -c https -P passphrase.txt`* .
5. Zadejte hello IP adresu a port:

   * Pokud se připojujete toohello konfigurační server přes internet zadejte hello konfigurace serveru virtuální veřejnou IP adresu a veřejný koncový bod HTTPS v hello `<IP address>` a `<port>`.
   * Pokud se připojujete prostřednictvím připojení VPN zadejte hello interní IP adresu a 443.

**toorun z příkazového řádku hello**:

1. Kopírovat heslo hello z hello CX toohello soubor "passphrase.txt" na serveru hello a spusťte tento příkazy. V našem příkladu CX i 104.40.75.37 a hello HTTPS port je 62519:

tooinstall na provozním serveru:

    ./install -t both -a host -R Agent -d /usr/local/ASR -i 104.40.75.37 -p 62519 -s y -c https -P passphrase.txt

tooinstall na cílovém serveru hello:

    ./install -t both -a host -R MasterTarget -d /usr/local/ASR -i 104.40.75.37 -p 62519 -s y -c https -P passphrase.txt

> [!NOTE]
> Když přidáte počítače tooa ochranné skupiny, která je již spuštěn příslušnou verzi hello služby Mobility, pak se přeskočí hello nabízenou instalaci.
>
>

## <a name="step-9-enable-protection"></a>Krok 9: Povolení ochrany
Ochrana tooenable přidáte virtuální počítače a skupiny ochrany tooa fyzických serverů. Než začnete, Všimněte si, že:

* Virtuální počítače jsou zjišťovány každých 15 minut a může to trvat až too15 minut, než je tooappear v Azure Site Recovery po zjišťování.
* Změny prostředí na virtuálním počítači hello (jako je třeba instalace nástroje VMware) může také trvat až toobe minut too15 aktualizovat ve službě Site Recovery.
* Můžete zkontrolovat hello poslední zjištěné čas v hello **poslední obraťte se na** pole pro hello vCenter server nebo hostiteli ESXi, na hello **konfigurační servery** stránky.
* Pokud jste již vytvořili skupinu ochrany a přidat vCenter Server nebo ESXi hostitele, po který, bude trvat 15 minut pro toorefresh portálu Azure Site Recovery hello a toobe virtuálních počítačů, které jsou uvedené v hello **skupiny ochrany tooa přidat počítače**  dialogové okno.
* Pokud chcete tooproceed okamžitě s přidávání počítače tooprotection skupiny bez čekání na hello naplánovaného zjišťování, zvýrazněte hello konfigurační server (nemáte klikněte na něj) a klikněte na tlačítko hello **aktualizovat** tlačítko.
* Když přidáte virtuální počítače nebo skupiny ochrany tooa fyzické počítače, server proces hello automaticky nabízených oznámení a nainstaluje hello služba Mobility na zdrojový server hello, pokud hello již není nainstalována.
* Mechanismus automatické nabízené hello toowork ujistěte, že jste nastavili chráněné počítače jak je popsáno v předchozím kroku hello.

Přidejte počítače následujícím způsobem:

1. Klikněte na tlačítko **chráněné položky** > **skupiny ochrany** > **počítače** > **přidat počítače** . Jako osvědčený postup doporučujeme, že by měl skupiny ochrany tak, aby přidat počítače, které běží konkrétní aplikaci toohello zrcadlení úlohy stejné skupiny.
2. V **vyberte virtuální počítače** Pokud chráníte fyzické servery, v hello **přidání fyzických počítačů** průvodce zadejte popisný název a hello IP adresu. Potom vyberte operační systém řady hello.

    ![Přidejte server V Center](./media/site-recovery-vmware-to-azure-classic-legacy/physical-protect.png)
3. V **vyberte virtuální počítače** Pokud chráníte virtuální počítače VMware, vyberte server vCenter, který spravuje virtuální počítače (nebo hello EXSi hostitele, na kterém používáte systém) a pak vyberte hello počítače.

    ![Přidejte server V Center](./media/site-recovery-vmware-to-azure-classic-legacy/select-vms.png)    
4. V **zadat cílové prostředky** vyberte hello hlavních cílových serverů a toouse úložiště pro replikaci a vyberte, zda text hello nastavení se použije pro všechny úlohy. Vyberte [prémiový účet úložiště](../storage/common/storage-premium-storage.md) při konfiguraci ochrany pro úlohy, které vyžadují konzistentní vysoké vstupně-výstupní výkon a nízká latence v pořadí toohost vstupně-výstupní operace náročné úlohy. Pokud chcete toouse účet úložiště Premium pro disky zatížení, je nutné toouse hello hlavního cíle DS-series. Disky úložiště Premium nelze použít s hlavního cíle DS řady.

   > [!NOTE]
   > Nepodporujeme hello přesun účty úložiště vytvořené pomocí hello [nový portál Azure](../storage/common/storage-create-storage-account.md) mezi skupinami prostředků.
   >
   >

    ![vCenter server](./media/site-recovery-vmware-to-azure-classic-legacy/machine-resources.png)
5. V **zadejte účty** vyberte hello účet chcete toouse pro instalaci služby Mobility hello na chráněných počítačích. přihlašovací údaje účtu Hello jsou potřebné pro automatickou instalaci hello služba Mobility. Pokud nemůžete vybrat účtu Ujistěte se, že nastavíte jeden jak je popsáno v kroku 2. Všimněte si, že tento účet není přístupný v Azure. Pro systém Windows server hello účet by měl mít oprávnění správce na zdrojovém serveru hello. Pro Linux musí být účet hello root.

    ![Přihlašovací údaje systému Linux](./media/site-recovery-vmware-to-azure-classic-legacy/mobility-account.png)
6. Klikněte na tlačítko toofinish zaškrtnutí hello přidání počítače toohello ochranu skupiny a toostart počáteční replikace pro každý počítač. Můžete sledovat stav na hello **úlohy** stránky.

    ![Přidejte server V Center](./media/site-recovery-vmware-to-azure-classic-legacy/pg-jobs2.png)
7. Kromě toho můžete sledovat stav ochrany kliknutím **chráněné položky** > název skupiny ochrany > **virtuální počítače** . Po dokončení počáteční replikace a hello počítače jsou synchronizaci dat se zobrazí **chráněné** stavu.

    ![Úlohy virtuálního počítače](./media/site-recovery-vmware-to-azure-classic-legacy/pg-jobs.png)

### <a name="set-protected-machine-properties"></a>Vlastnosti sady chráněné počítače
1. Jakmile počítač má **chráněné** stav, můžete konfigurovat její vlastnosti převzetí služeb při selhání. V podrobnostech skupiny ochrany hello vyberte hello počítače a otevřete hello **konfigurace** kartě.
2. Můžete upravit hello název, který bude mu udělená toohello počítače v Azure po převzetí služeb při selhání a hello velikost virtuálního počítače Azure. Můžete také vybrat hello síť Azure toowhich hello počítač připojí po převzetí služeb při selhání.

   > [!NOTE]
   > [Migrace sítí](../resource-group-move-resources.md) napříč prostředků skupiny v hello stejného předplatného, nebo ve předplatných není podporována pro sítě používá pro nasazení Site Recovery.
   >
   >

    ![Nastavit vlastnosti virtuálního počítače](./media/site-recovery-vmware-to-azure-classic-legacy/vm-props.png)

Poznámky:

* Název Hello hello počítače Azure musí být v souladu s požadavky na Azure.
* Ve výchozím nastavení replikované virtuální počítače v Azure nejsou připojené tooan síť Azure. Pokud chcete, aby replikovaných virtuálních počítačů, zajistěte, aby toocommunicate tooset hello stejnou síť Azure pro ně.
* Pokud změníte velikost svazku na virtuální počítač VMware nebo fyzický server přejde do kritického stavu. Pokud potřebujete toomodify hello velikost, hello následující:

  * (a) hello velikost nastavení změnit.
  * b) v hello **virtuální počítače** vyberte hello virtuální počítač a klikněte na tlačítko **odebrat**.
  * c) v **odebrat virtuální počítač** možnost hello **zakažte ochranu (slouží pro a změnu velikosti svazku)**. Tato možnost zakáže ochranu, ale zachová body obnovení hello v Azure.

      ![Nastavit vlastnosti virtuálního počítače](./media/site-recovery-vmware-to-azure-classic-legacy/remove-vm.png)
  * d) znovu povolte ochranu pro virtuální počítač hello. Když znovu povolit ochranu dat hello hello po změně velikosti svazku, bude přenášená tooAzure.

## <a name="step-10-run-a-failover"></a>Krok 10: Spuštění převzetí služeb při selhání
Aktuálně lze spustit jen neplánované převzetí služeb při selhání pro chráněné virtuální počítače VMware a fyzické servery. Vezměte na vědomí následující hello:

* Před spuštěním převzetí služeb při selhání, zajistěte, aby byly hello konfigurace a hlavním cílovým serverům běží a je v pořádku. V opačném případě převzetí služeb při selhání se nezdaří.
* Zdrojového počítače nejsou vypnuté jako součást neplánované převzetí služeb při selhání. Provádění neplánované převzetí služeb při selhání se zastaví replikaci dat pro servery hello chráněný. Budete potřebovat toodelete hello počítače ze skupiny ochrany hello a znovu přidejte v pořadí toostart znovu ochranu počítačů po dokončení technologie hello neplánované převzetí služeb při selhání.
* Pokud chcete toofail přes bez ztráty dat, ujistěte se, že virtuálních počítačích primární lokality hello jsou vypnuty před spuštěním převzetí služeb při selhání hello.

1. Na hello **plány obnovení** stránky a přidejte plán obnovení. Zadejte podrobnosti pro plán hello a vyberte **Azure** jako cíl hello.

    ![Konfigurace plánu obnovení](./media/site-recovery-vmware-to-azure-classic-legacy/rplan1.png)
2. V **vybrat virtuální počítač** vyberte skupinu ochrany a pak vyberte počítače v plánu obnovení toohello tooadd skupiny hello. [Další informace](site-recovery-create-recovery-plans.md) o plány obnovení.

    ![Přidat virtuální počítače](./media/site-recovery-vmware-to-azure-classic-legacy/rplan2.png)
3. V případě potřeby můžete přizpůsobit hello plán toocreate skupin a pořadí hello pořadí na počítačích, které v hello obnovení plánu jsou převzetí služeb při selhání. Můžete také přidat vyzve k zadání ručně prováděné akce a skripty. Při obnovování tooAzure lze přidat pomocí Hello skripty [sad Azure Automation Runbook](site-recovery-runbook-automation.md).
4. V hello **plány obnovení** stránku hello vyberte plán a klikněte na tlačítko **neplánované převzetí služeb při selhání**.
5. V **potvrzení převzetí služeb při selhání** ověřte hello směr převzetí služeb při selhání (tooAzure) a vyberte toofail bodu obnovení hello přes.
6. Počkejte toocomplete úlohy hello převzetí služeb při selhání a potom ověřte, zda převzetí služeb při selhání hello fungovala podle očekávání a že hello replikovat virtuální počítače spustit úspěšně v Azure.

## <a name="step-11-fail-back-failed-over-machines-from-azure"></a>Krok 11: Selhání zpět převzetí služeb při selhání počítačů z Azure
[Další informace](site-recovery-failback-azure-to-vmware-classic-legacy.md) o toobring vaše neúspěšné přes počítače, které běží v Azure zpět tooyour v místním prostředí.

## <a name="manage-your-process-servers"></a>Spravovat vaše servery procesu
Hello procesový server odešle replikace dat toohello hlavní cílový server v Azure a zjišťuje nový server vCenter přidané tooa virtuálních počítačů VMware. V následujících případech hello můžete ve vašem nasazení toochange hello procesového serveru:

* Pokud aktuální proces server hello ocitne mimo provoz
* Pokud váš bodu obnovení cíl (RPO) roste tooan nepřijatelné úroveň pro vaši organizaci.

V případě potřeby, že můžete přesunout replikaci hello některé nebo všechny vaše místní VMware virtuální počítače a fyzické servery tooa různé zpracovat serveru. Například:

* **Selhání**– Pokud procesový server selže nebo není k dispozici můžete přesunout chráněný počítač replikace tooanother procesový server. Metadata hello zdrojového počítače a počítač repliky se přesunutý toohello nový procesový server a je-li synchronizovat data. Hello nový procesový server se automaticky připojí toohello vCenter server tooperform automatického zjišťování. Můžete monitorovat stav hello proces serverů na řídicím panelu hello Site Recovery.
* **Tooadjust RPO Vyrovnávání zatížení**– pro vylepšené můžete Vyrovnávání zatížení můžete vybrat jiný procesový server na portálu Site Recovery hello a přesuňte replikaci jeden nebo více počítačů tooit pro vyrovnávání zatížení ruční. V takovém případě metadata hello vybraný zdroj a počítače repliky jsou přesunutý toohello nový procesový server. původní procesový server Hello zůstává připojený toohello vCenter server.

### <a name="monitor-hello-process-server"></a>Monitorování hello procesového serveru
Pokud procesový server je v kritickém stavu stavu upozornění, zobrazí se na hello řídicího panelu pro obnovení lokality. Můžete kliknutím na kartu události hello stav tooopen hello a pak podrobně toospecific úloh na kartě úlohy hello.

### <a name="modify-hello-process-server-used-for-replication"></a>Upravit hello procesový server používají pro replikaci
1. Otevřete **servery** > **konfigurační servery** > konfigurační server > **podrobnosti o serveru**.
2. Klikněte na tlačítko **proces servery** > **změnit procesový Server** další server toohello chcete toomodify.

    ![Změnit procesový Server 1](./media/site-recovery-vmware-to-azure-classic-legacy/change-ps1.png)
3. V **změnit procesový Server** > **cílový procesový Server** vyberte hello nový server budete toouse a pak vyberte hello virtuální počítače, který má tooreplicate toohello nový server. Klikněte na tlačítko hello informace ikonu další toohello název serveru podrobnosti volného místa a využité paměti. Hello průměrná prostor, který bude požadované tooreplicate každý vybraný virtuální počítač toohello nový procesový server je zobrazené toohelp provedené načíst rozhodnutí.

    ![Změnit procesový Server 2](./media/site-recovery-vmware-to-azure-classic-legacy/change-ps2.png)
4. Klikněte na tlačítko toobegin zaškrtnutí hello replikace toohello nový procesový server. Všimněte si, že když odeberete všechny virtuální počítače z procesového serveru, který byl kritické ho už zobrazí kritická upozornění na řídicím panelu hello.

## <a name="third-party-software-notices-and-information"></a>Oznámení software třetích stran a informace
Nechcete převede nebo lokalizaci

Hello software a firmware spuštěné v hello produktů společnosti Microsoft nebo služba je založena na nebo zahrnuje materiálu z hello projekty uvedené níže (dále souhrnně nazývané "kód třetí strany").  Společnost Microsoft se hello není původní autor hello kód třetích stran.  Hello původní autorská práva a licenci, pod kterým společnost Microsoft takový kód třetích stran, jsou nastavené níže uvedenými.

Hello informace v části A týká níže uvedené součásti z projektů hello kód třetích stran. Tyto licence a informace jsou uvedené pro pouze pro informační účely.  Tento kód třetích stran, která má být relicensed tooyou společností Microsoft v rámci licenční podmínky pro produkty Microsoft hello nebo služby společnosti Microsoft software.  

Hello informace v části B je týkající se součástí kód třetích stran, které jsou určené k dispozici tooyou společností Microsoft v rámci hello původní licenční podmínky.

Hello celého souboru může být nalezen na hello [Microsoft Download Center](http://go.microsoft.com/fwlink/?LinkId=529428). Společnost Microsoft si vyhrazuje všechna práva, která nejsou výslovně udělena, zda implicitně, jako překážku uplatnění nároku či jiným způsobem.
