---
title: "aaaExclude disky z ochrany pomocí Azure Site Recovery | Microsoft Docs"
description: "Popisuje, proč a jak tooexclude virtuálních počítačů disky z replikace pro scénáře VMware tooAzure a tooAzure technologie Hyper-V."
services: site-recovery
documentationcenter: 
author: nsoneji
manager: garavd
editor: 
ms.assetid: 
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 06/05/2017
ms.author: nisoneji
ms.openlocfilehash: f47146bc57aeab3fce90123d0894fa86dde93417
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="exclude-disks-from-replication"></a>Vyloučení disků z replikace
Tento článek popisuje, jak tooexclude disky z replikace. Toto vyloučení můžete optimalizovat replikace hello využívat šířku pásma nebo optimalizovat hello straně cíle prostředky, které využívají tyto disky. Funkce Hello je podporována pro scénáře VMware tooAzure a tooAzure technologie Hyper-V.

## <a name="prerequisites"></a>Požadavky

Ve výchozím nastavení se replikují všechny disky virtuálního počítače. tooexclude disku z replikace, musíte ručně nainstalovat hello služba Mobility na počítači hello předtím, než povolíte replikaci, pokud replikujete z VMware tooAzure.


## <a name="why-exclude-disks-from-replication"></a>Proč vylučovat disky z replikace?
Vyloučení disků z replikace je často nutné z těchto důvodů:

- Hello data, která je na disku hello vyloučené churned není důležité nebo nepotřebuje toobe replikovat.

- Chcete toosave úložiště a síťové prostředky podle nereplikuje tato změn.

## <a name="what-are-hello-typical-scenarios"></a>Jaké jsou typické scénáře hello?
Můžete určit konkrétní příklady častých změn dat, které jsou skvělými kandidáty na vyloučení. Příklady mohou zahrnovat zapíše tooa stránkovací soubor (pagefile.sys) a zapíše soubor databáze tempdb toohello systému Microsoft SQL Server. V závislosti na zatížení hello a hello subsystému úložiště můžete zaregistrovat hello stránkovacího souboru významné množství změn. Tato data replikace z primární lokality tooAzure hello by však být náročné. Proto můžete použít následující kroky toooptimize replikace virtuálního počítače s jeden virtuální disk, který má hello operační systém a stránkovací soubor hello hello:

1. Rozdělit hello jeden virtuální disk na dva virtuální disky. Jeden virtuální disk má hello operační systém a hello jiných má hello stránkovacího souboru.
2. Z replikace vylučte disku se souborem hello stránkování.

Podobně můžete použít následující kroky toooptimize disk, který má obě databáze tempdb systému Microsoft SQL Server hello souboru hello a hello soubor systémové databáze:

1. Zachovat hello systémovou databázi a databázi tempdb na dvou různých disků.
2. Z replikace vylučte hello databáze tempdb disku.

## <a name="how-tooexclude-disks-from-replication"></a>Jak tooexclude disky z replikace?

### <a name="vmware-tooazure"></a>VMware tooAzure
Postupujte podle hello [povolit replikaci](site-recovery-vmware-to-azure.md) tooprotect pracovního postupu pro virtuální počítač z portálu Azure Site Recovery hello. V hello čtvrtý krok hello pracovního postupu, použijte hello **tooREPLICATE disku** sloupec tooexclude disky z replikace. Ve výchozím nastavení jsou pro replikaci vybrány všechny disky. Zrušte zaškrtnutí políčka hello disků, které chcete tooexclude z replikace a pak dokončení hello kroky tooenable replikace.

![Z replikace vyloučit disky a povolení replikace pro navrácení služeb po obnovení VMware tooAzure](./media/site-recovery-exclude-disk/v2a-enable-replication-exclude-disk1.png)


>[!NOTE]
>
> * Můžete vyloučit jenom ty disky, které už máte nainstalovaná služba Mobility hello. Služba Mobility hello toomanually instalace, je nutné, protože hello služba Mobility je nainstalován pouze pomocí mechanizmu nabízené hello po povolení replikace.
> * Z replikace můžete vyloučit pouze běžné disky. Nemůžete vyloučit disk operačního systému ani dynamické disky.
> * Po povolení replikace už není možné přidávat nebo odebírat disky pro replikaci. Chcete-li tooadd nebo vyloučit disk, můžete potřebovat toodisable ochrany pro počítač hello a pak ji znovu povolte.
> * Pokud vyloučíte disk, který je potřeba pro toooperate aplikaci po převzetí služeb při selhání tooAzure, budete potřebovat disk hello toocreate ručně v Azure tak, aby bylo možné spouštět aplikace hello replikovat. Alternativně můžete integrovat Azure automation na disk hello toocreate plán obnovení během převzetí služeb při selhání hello počítače.
> * Virtuální počítače s Windows: Disky, které ručně vytvoříte v Azure, nebude možné po navrácení služeb obnovit. Například když při selhání převezmete tři disky a pak přímo ve službě Azure Virtual Machines vytvoříte další dva, při navrácení služeb po obnovení se přenesou jen tři disky replikované při selhání. Nesmí obsahovat disky, které jste vytvořili ručně v navrácení služeb po obnovení nebo opětovné ochrany z místní tooAzure.
> * Virtuální počítače s Linuxem: Disky, které ručně vytvoříte v Azure, se po navrácení služeb obnoví. Například když při selhání převezmete tři disky a pak přímo ve službě Azure Virtual Machines vytvoříte další dva, při navrácení služeb se obnoví všech pět. Ručně vytvořené disky nemůžete vyloučit z navrácení služeb po obnovení.
>

### <a name="hyper-v-tooazure"></a>TooAzure technologie Hyper-V
Postupujte podle hello [povolit replikaci](site-recovery-hyper-v-site-to-azure.md) tooprotect pracovního postupu pro virtuální počítač z portálu Azure Site Recovery hello. V hello čtvrtý krok hello pracovního postupu, použijte hello **tooREPLICATE disku** sloupec tooexclude disky z replikace. Ve výchozím nastavení jsou pro replikaci vybrány všechny disky. Zrušte zaškrtnutí políčka hello disků, které chcete tooexclude z replikace a pak dokončení hello kroky tooenable replikace.

![Z replikace vyloučit disky a povolení replikace pro navrácení služeb po obnovení technologie Hyper-V tooAzure](./media/site-recovery-vmm-to-azure/enable-replication6-with-exclude-disk.png)

>[!NOTE]
>
> * Z replikace můžete vyloučit pouze běžné disky. Nemůžete vyloučit disky operačního systému. Doporučujeme, abyste nevylučovali dynamické disky. Azure Site Recovery nemůže identifikovat, které virtuální pevný disk (VHD) je základní nebo dynamické v hello hostovaného virtuálního počítače.  Pokud nejsou všechny disky, závislé dynamický svazek vyloučené, hello chráněné dynamického disku se změní na disku na virtuálním počítači převzetí služeb při selhání, který selhal a hello dat na disk není dostupný.
> * Po povolení replikace už není možné přidávat nebo odebírat disky pro replikaci. Chcete-li tooadd nebo vyloučit disk, potřebujete toodisable ochranu pro virtuální počítač hello a pak ji znovu povolte.
> * Pokud vyloučíte disk, který je potřeba pro toooperate aplikaci, po převzetí služeb při selhání tooAzure budete potřebovat disk hello toocreate ručně v Azure tak, aby bylo možné spouštět aplikace hello replikovat. Alternativně můžete integrovat Azure automation na disk hello toocreate plán obnovení během převzetí služeb při selhání hello počítače.
> * Disky, které ručně vytvoříte v Azure, nebude možné po obnovení navrátit. Například pokud nezdaří tři disky a vytvořit dva disky přímo v Azure Virtual Machines, jenom tři disky, které byly převzetí služeb při selhání se nepodařilo zpět z Azure tooHyper-V. Nesmí obsahovat disky, které byly vytvořeny ručně v navrácení služeb po obnovení, nebo v zpětná replikace z tooAzure technologie Hyper-V.



## <a name="end-to-end-scenarios-of-exclude-disks"></a>Úplné scénáře vyloučení disků
Zvažte dva scénáře toounderstand hello vyloučení disku funkce:

- Disk s databází tempdb systému SQL Server
- Disk se stránkovacím souborem (pagefile.sys)

### <a name="exclude-hello-sql-server-tempdb-disk"></a>Vyloučení disku databáze tempdb systému SQL Server hello
Uvažujme virtuální počítač se systémem SQL Server, který používá databázi tempdb, kterou chcete vyloučit z replikace.

Název Hello hello virtuálního disku je SalesDB.

Disky na hello zdrojového virtuálního počítače jsou následující:


**Název disku** | **Označení disku v hostovaném operačním systému** | **Písmeno jednotky** | **Typ dat na disku hello**
--- | --- | --- | ---
DB-Disk0-OS | DISK0 | C:\ | Disk operačním systému
DB-Disk1| Disk1 | D:\ | Databáze systému SQL a uživatelská databáze 1
DB-Disk2 (disk hello vyloučené z ochrany) | Disk2 | E:\ | Dočasné soubory
DB-Disk3 (disk hello vyloučené z ochrany) | Disk3 | F:\ | SQL databáze tempdb (cesta ke složce (F:\MSSQL\Data\) < /br/>< /br/> zapište cestu ke složce hello před převzetí služeb při selhání.
DB Disk4 | Disk4 |G:\ |Uživatelská databáze 2

Protože změn dat na dva disky hello virtuálního počítače je dočasný, když chráníte hello SalesDB virtuálního počítače, Disk2 a Disk3 vylučte z replikace. Azure Site Recovery nebude tyto disky replikovat. Na převzetí služeb při selhání nemusí být tyto disky na hello převzetí služeb při selhání virtuálního počítače na platformě Azure.

Disky na virtuální počítač Azure po převzetí služeb při selhání hello jsou následující:

**Označení disku v hostovaném operačním systému** | **Písmeno jednotky** | **Typ dat na disku hello**
--- | --- | ---
DISK0 | C:\ | Disk operačním systému
Disk1 | E:\ | Dočasné úložiště < /br / >< /br / > Azure přidá tento disk a přiřadí hello první dostupné písmeno jednotky.
Disk2 | D:\ | Databáze systému SQL a uživatelská databáze 1
Disk3 | G:\ | Uživatelská databáze 2

Protože Disk2 a Disk3 byly vyloučeny z hello SalesDB virtuálního počítače, je E: hello první písmeno jednotky ze seznamu dostupných hello. Azure přiřadí svazku E: toohello dočasné úložiště. Pro všechny disky hello replikovat hello hello jednotku, kterou písmena zůstávají stejné.

Disk3, která byla hello SQL databáze tempdb disku (cesta ke složce databáze tempdb F:\MSSQL\Data\), byla z replikace vyloučit. Hello disk není k dispozici na hello převzetí služeb při selhání virtuálního počítače. V důsledku toho hello služba SQL je v zastaveném stavu a je nutné hello F:\MSSQL\Data cesta.

Existují dva způsoby toocreate tuto cestu:

- Přidejte nový disk a přiřaďte mu cestu ke složce databáze tempdb.
- Použijte stávající disk dočasné úložiště pro cestu ke složce databáze tempdb hello.

#### <a name="add-a-new-disk"></a>Přidání nového disku:

1. Poznamenejte si hello cesty SQL tempdb.mdf a tempdb.ldf před převzetí služeb při selhání.
2. Z hello portálu Azure přidáte nový disk toohello převzetí služeb při selhání virtuální počítač s hello stejné nebo další velikost jako hello zdroje SQL databáze tempdb disku (Disk3).
3. Přihlaste se toohello virtuální počítač Azure. Z konzoly pro správu (diskmgmt.msc) disku hello inicializovat a formát hello nově přidaná disku.
4. Hello přiřadit stejné písmeno, který byl používán hello SQL databáze tempdb disku (F:) jednotky.
5. Vytvořte složku databáze tempdb na hello svazku F: (F:\MSSQL\Data).
6. Spusťte službu SQL hello z konzoly služby hello.

#### <a name="use-an-existing-temporary-storage-disk-for-hello-sql-tempdb-folder-path"></a>Použijte stávající disk dočasné úložiště pro cestu ke složce hello SQL databáze tempdb:

1. Otevřete příkazový řádek.
2. Spusťte systém SQL Server v režimu obnovení z příkazového řádku hello.

        Net start MSSQLSERVER /f / T3608

3. Spusťte hello následující sqlcmd toochange hello databáze tempdb cesta toohello novou cestu.

        sqlcmd -A -S SalesDB        **Use your SQL DBname**
        USE master;     
        GO      
        ALTER DATABASE tempdb       
        MODIFY FILE (NAME = tempdev, FILENAME = 'E:\MSSQL\tempdata\tempdb.mdf');
        GO      
        ALTER DATABASE tempdb       
        MODIFY FILE (NAME = templog, FILENAME = 'E:\MSSQL\tempdata\templog.ldf');       
        GO


4. Zastavte službu Microsoft SQL Server hello.

        Net stop MSSQLSERVER
5. Spusťte službu Microsoft SQL Server hello.

        Net start MSSQLSERVER

Odkažte toohello následující Azure platí pro dočasné úložiště disku:

* [Pomocí SSD disky ve virtuálních počítačích Azure toostore databáze TempDB systému SQL Server a rozšíření fondu vyrovnávací paměti](https://blogs.technet.microsoft.com/dataplatforminsider/2014/09/25/using-ssds-in-azure-vms-to-store-sql-server-tempdb-and-buffer-pool-extensions/)
* [Osvědčené postupy z hlediska výkonu pro SQL Server na Azure Virtual Machines](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-performance)

### <a name="failback-from-azure-tooan-on-premises-host"></a>Navrácení služeb po obnovení (z Azure tooan místní hostitel)
Nyní Pojďme pochopit hello disky, které jsou replikované, kdy jste převzetí služeb při selhání z Azure tooyour místní VMware nebo technologie Hyper-V v hostiteli. Disky, které vytvoříte v Azure ručně, se replikovat nebudou. Například když při selhání převezmete tři disky a pak přímo ve službě Azure Virtual Machines vytvoříte další dva, při navrácení služeb po obnovení se přenesou jen tři disky replikované při selhání. Nesmí obsahovat disky, které byly vytvořeny ručně v navrácení služeb po obnovení nebo opětovné ochrany z místní tooAzure. Je také se nereplikuje hello dočasné úložiště disku tooon místního hostitele.

#### <a name="failback-toooriginal-location-recovery"></a>Obnovení do umístění toooriginal navrácení služeb po obnovení

V předchozím příkladu hello konfigurace disku virtuálního počítače Azure hello vypadá takto:

**Označení disku v hostovaném operačním systému** | **Písmeno jednotky** | **Typ dat na disku hello**
--- | --- | ---
DISK0 | C:\ | Disk operačním systému
Disk1 | E:\ | Dočasné úložiště < /br / >< /br / > Azure přidá tento disk a přiřadí hello první dostupné písmeno jednotky.
Disk2 | D:\ | Databáze systému SQL a uživatelská databáze 1
Disk3 | G:\ | Uživatelská databáze 2


#### <a name="vmware-tooazure"></a>VMware tooAzure
Pokud navrácení služeb po obnovení se provádí toohello původního umístění, konfigurace disku virtuálního počítače navrácení služeb po obnovení hello nemá vyloučené disky. Disky, které byly vyloučeny z VMware tooAzure nebudete mít k dispozici na hello navrácení služeb po obnovení virtuálního počítače.

Po plánovaném převzetí služeb při selhání Azure tooon místní VMware jsou disky na virtuální počítač VMWare hello (původní umístění):

**Označení disku v hostovaném operačním systému** | **Písmeno jednotky** | **Typ dat na disku hello**
--- | --- | ---
DISK0 | C:\ | Disk operačním systému
Disk1 | D:\ | Databáze systému SQL a uživatelská databáze 1
Disk2 | G:\ | Uživatelská databáze 2

#### <a name="hyper-v-tooazure"></a>TooAzure technologie Hyper-V
Při navrácení služeb po obnovení je toohello původního umístění, hello navrácení služeb po obnovení virtuálního počítače disku konfigurace zůstanou hello stejné jako původní konfigurace disku virtuálního počítače pro technologii Hyper-V. Disky, které byly vyloučeny z technologie Hyper-V lokality tooAzure jsou k dispozici na hello navrácení služeb po obnovení virtuálního počítače.

Po plánovaném převzetí služeb při selhání Azure tooon místní technologie Hyper-V jsou disky na virtuální počítače Hyper-V hello (původní umístění):

**Název disku** | **Označení disku v hostovaném operačním systému** | **Písmeno jednotky** | **Typ dat na disku hello**
--- | --- | --- | ---
DB-Disk0-OS | DISK0 |   C:\ | Disk operačním systému
DB-Disk1 | Disk1 | D:\ | Databáze systému SQL a uživatelská databáze 1
DB-Disk2 (vyloučený disk) | Disk2 | E:\ | Dočasné soubory
DB-Disk3 (vyloučený disk) | Disk3 | F:\ | SQL databáze tempdb (cesta ke složce (F:\MSSQL\Data\)
DB Disk4 | Disk4 | G:\ | Uživatelská databáze 2


#### <a name="exclude-hello-paging-file-pagefilesys-disk"></a>Vyloučení disku se souborem (pagefile.sys) hello stránkování

Uvažujme virtuální počítač s diskem pro stránkovací soubor, který je možné vyloučit z replikace.
Existují dva možné případy.

#### <a name="case-1-hello-paging-file-is-configured-on-hello-d-drive"></a>Případ 1: hello stránkovacího souboru je nakonfigurovaná na hello jednotky D:
Tady je hello konfigurace disku:


**Název disku** | **Označení disku v hostovaném operačním systému** | **Písmeno jednotky** | **Typ dat na disku hello**
--- | --- | --- | ---
DB-Disk0-OS | DISK0 | C:\ | Disk operačním systému
DB-disk 1 (disk hello vyloučené z ochrany hello) | Disk1 | D:\ | pagefile.sys
DB-Disk2 | Disk2 | E:\ | Uživatelská data 1
DB-Disk3 | Disk3 | F:\ | Uživatelská data 2

Zde jsou nastavení hello stránkovacího souboru na hello zdrojového virtuálního počítače:

![Nastavení stránkovacího souboru na zdrojovém virtuálním počítači](./media/site-recovery-exclude-disk/pagefile-on-d-drive-sourceVM.png)


Disky na hello virtuálního počítače Azure po převzetí služeb při selhání hello virtuálního počítače z VMware tooAzure nebo tooAzure technologie Hyper-V, jsou následující:

**Název disku** | **Označení disku v hostovaném operačním systému** | **Písmeno jednotky** | **Typ dat na disku hello**
--- | --- | --- | ---
DB-Disk0-OS | DISK0 | C:\ | Disk operačním systému
DB-Disk1 | Disk1 | D:\ | Dočasné úložiště</br /> </br />pagefile.sys
DB-Disk2 | Disk2 | E:\ | Uživatelská data 1
DB-Disk3 | Disk3 | F:\ | Uživatelská data 2

Protože se vyloučila disk 1 (D:), je D: hello první písmeno jednotky ze seznamu dostupných hello. Azure přiřadí svazku D: toohello dočasné úložiště. Protože D: je dostupný na hello virtuální počítač Azure, hello hello stránkovacího souboru z hello virtuální počítač zůstane stejný.

Zde jsou nastavení hello stránkovacího souboru na hello virtuální počítač Azure:

![Nastavení stránkovacího souboru na virtuálním počítači Azure](./media/site-recovery-exclude-disk/pagefile-on-Azure-vm-after-failover.png)

#### <a name="case-2-hello-paging-file-is-configured-on-another-drive-other-than-d-drive"></a>Případ 2: hello stránkovacího souboru je nakonfigurovaná na jinou jednotku (jiné než jednotka D:)

Tady je konfigurace disku virtuálního počítače zdrojového hello:

**Název disku** | **Označení disku v hostovaném operačním systému** | **Písmeno jednotky** | **Typ dat na disku hello**
--- | --- | --- | ---
DB-Disk0-OS | DISK0 | C:\ | Disk operačním systému
DB-disk 1 (disk hello vyloučené z ochrany) | Disk1 | G:\ | pagefile.sys
DB-Disk2 | Disk2 | E:\ | Uživatelská data 1
DB-Disk3 | Disk3 | F:\ | Uživatelská data 2

Zde jsou nastavení hello stránkovacího souboru na hello místní virtuální počítač:

![Stránkovací soubor nastavení na hello místní virtuální počítač](./media/site-recovery-exclude-disk/pagefile-on-g-drive-sourceVM.png)

Disky na hello virtuálního počítače Azure po převzetí služeb při selhání hello virtuálního počítače z tooAzure VMware nebo Hyper-V, jsou následující:

**Název disku**| **Označení disku v hostovaném operačním systému**| **Písmeno jednotky** | **Typ dat na disku hello**
--- | --- | --- | ---
DB-Disk0-OS | DISK0  |C:\ |Disk operačním systému
DB-Disk1 | Disk1 | D:\ | Dočasné úložiště</br /> </br />pagefile.sys
DB-Disk2 | Disk2 | E:\ | Uživatelská data 1
DB-Disk3 | Disk3 | F:\ | Uživatelská data 2

Vzhledem k tomu, že D: hello první písmeno jednotky ze seznamu dostupných hello se Azure přiřadí svazku D: toohello dočasné úložiště. Pro všechny disky hello replikovat zůstane písmeno jednotky hello hello stejné. Protože hello G: disku není k dispozici, bude systém hello používat jednotky C: hello hello stránkovací soubor.

Zde jsou nastavení hello stránkovacího souboru na hello virtuální počítač Azure:

![Nastavení stránkovacího souboru na virtuálním počítači Azure](./media/site-recovery-exclude-disk/pagefile-on-Azure-vm-after-failover-2.png)

## <a name="next-steps"></a>Další kroky
Po nasazení a zprovoznění nasazení si můžete přečíst [další informace](site-recovery-failover.md) o různých typech převzetí služeb při selhání.
