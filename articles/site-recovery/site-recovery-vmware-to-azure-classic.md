---
title: "aaaReplicate virtuální počítače VMware a fyzické servery tooAzure v hello klasického portálu | Microsoft Docs"
description: "Tento článek popisuje, jak toodeploy Azure Site Recovery tooorchestrate replikace, převzetí služeb při selhání a obnovení místní virtuální počítače VMware a fyzické servery tooAzure Windows nebo Linuxem."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: jwhit
editor: 
ms.assetid: a9022c1f-43c1-4d38-841f-52540025fb46
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/23/2017
ms.author: raynew
ROBOTS: NOINDEX, NOFOLLOW
redirect_url: site-recovery-vmware-to-azure
ms.openlocfilehash: f85e4139ad45552ce963072e14d71d279bb7dac9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="replicate-vmware-virtual-machines-and-physical-servers-tooazure-with-azure-site-recovery"></a>Replikovat virtuální počítače VMware a fyzické servery tooAzure s Azure Site Recovery
> [!div class="op_single_selector"]
> * [Hello portálu Azure](site-recovery-vmware-to-azure.md)
> * [portál classic Hello](site-recovery-vmware-to-azure-classic.md)
> * [portál classic Hello (zastaralé)](site-recovery-vmware-to-azure-classic-legacy.md)
>
>

Hello služba Azure Site Recovery přispívá tooyour provozní kontinuitu a strategie po havárii (BCDR) tím, že orchestruje replikaci, převzetí služeb při selhání a obnovení virtuálních počítačů a fyzických serverů. Počítače můžou být replikované tooAzure nebo tooa sekundárního místního datového centra. Rychlý přehled, najdete v tématu [co je Azure Site Recovery?](site-recovery-overview.md).

## <a name="overview"></a>Přehled
Tento článek popisuje, jak:

* **Replikovat tooAzure virtuální počítače VMware**: nasazení Site Recovery toocoordinate replikace, převzetí služeb při selhání a obnovení úložiště tooAzure virtuálních počítačů VMware na místě.
* **Replikace fyzických serverů tooAzure**: nasazení Azure Site Recovery toocoordinate replikace, převzetí služeb při selhání a obnovení místního fyzického systému Windows a Linux tooAzure servery.

> [!NOTE]
> Tento článek popisuje, jak tooreplicate tooAzure. Pokud chcete tooreplicate virtuální počítače VMware nebo Windows nebo Linuxem fyzických serverů tooa sekundárního datacentra, najdete v části [VMware obnovení lokality tooVMware](site-recovery-vmware-to-vmware.md).
>
>

POST jakékoli dotazy nebo připomínky v hello dolní části tohoto článku nebo na hello [fóru Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

## <a name="enhanced-deployment"></a>Rozšířeného nasazení
Tento článek obsahuje pokyny pro rozšířeného nasazení v hello portál Azure classic. Doporučujeme použít tuto verzi pro všechna nová nasazení. Pokud jste už nasazená ve pomocí hello starší starší verze, doporučujeme migraci toohello novou verzi. Další informace o migraci najdete v tématu [VMware obnovení lokality tooAzure classic starší verze](site-recovery-vmware-to-azure-classic-legacy.md#migrate-to-the-enhanced-deployment).

hlavní aktualizace se Hello rozšířeného nasazení. Zde je souhrn hello vylepšení, které jsme provedli jsme:

* **Bez infrastruktury virtuálních počítačů v Azure**: Data se replikují přímo tooan účtu úložiště Azure. Kromě toho není bez nutnosti tooset si jakékoliv infrastruktury virtuálních počítačů (např. konfigurační server nebo hlavní cílový server) pro replikaci a převzetí služeb při selhání potřeby se v nasazení hello starší verze.  
* **Unified instalace**: jedna instalace poskytuje jednoduché nastavení a škálovatelnost pro místní komponenty.
* **Zabezpečit nasazení**: veškerý provoz je zašifrován a komunikaci správu replikace se odesílají přes protokol HTTPS 443.
* **Body obnovení**: podpora pro selhání a obnovení konzistentních s aplikací body pro Windows a Linux prostředí a podpora pro obě jedna konfigurace virtuálních počítačů a více virtuálních počítačů konzistentní.
* **Testovací převzetí služeb při selhání**: podpora pro tooAzure omezovaly testovací převzetí služeb při selhání bez dopadu na produkční prostředí nebo pozastavení replikace.
* **Neplánované převzetí služeb při selhání**: podpora pro neplánované převzetí služeb při selhání tooAzure s tooautomatically rozšířené možnost vypnout virtuální počítače před převzetí služeb při selhání.
* **Navrácení služeb po obnovení**: integrované navrácení služeb po obnovení, která se replikují jenom rozdílové změny zpět toohello místní lokality.
* **vSphere 6.0**: omezenou podporu pro nasazení VMware vSphere 6.0.

## <a name="how-does-site-recovery-help-protect-virtual-machines-and-physical-servers"></a>Jak Site Recovery pomáhá chránit virtuální počítače a fyzické servery?
* VMware správci mohou nakonfigurovat tooprotect odlehlého ochrana Azure z firemní úlohy a aplikace běžící na virtuálních počítačů VMware. Správce serveru můžete replikovat tooAzure servery fyzický místní systém Windows a Linux.
* Hello konzoly Azure Site Recovery poskytuje jedno umístění pro jednoduché nastavení a správu replikace, převzetí služeb při selhání a obnovení procesy.
* Pokud budete replikovat virtuální počítače VMware, které jsou spravovány nástrojem vCenter server, Site Recovery může zjišťovat těchto virtuálních počítačů. Pokud počítače na hostiteli ESXi, Site Recovery vyhledá virtuální počítače na hostiteli hello.
* Pokud spustíte z vaší místní infrastruktury tooAzure snadné převzetí služeb při selhání, můžete se nezdaří zálohování (obnovení) ze serverů virtuálních počítačů Azure tooVMware hello místního webu.
* Můžete nakonfigurovat plány obnovení, které skupiny úloh aplikace, které jsou vrstvené napříč více počítačů. Pokud jste převzetí služeb při selhání tyto plány, Site Recovery poskytuje konzistence více virtuálních počítačů, aby počítače, které běží hello, který může být stejné úlohy obnovit společně tooa konzistentní datového bodu.

## <a name="supported-operating-systems"></a>Podporované operační systémy
### <a name="windows-64-bit-only"></a>Windows (pouze 64bitová verze)
* Windows Server 2008 R2 SP1 +
* Windows Server 2012
* Windows Server 2012 R2

### <a name="linux-64-bit-only"></a>Linux (pouze 64bitová verze)
* Red Hat Enterprise Linux 7.2, 6.7 a 7.1
* CentOS verze 6.5, 6.6, 6.7, 7.0, 7.1 a 7.2
* Oracle Linux Enterprise 6.4 a 6.5 systémem buď hello Red Hat kompatibilní jádra nebo nedělitelné Enterprise jádra verze 3 (UEK3)
* SUSE Linux Enterprise Server 11 SP3

## <a name="scenario-architecture"></a>Architektura scénáře
Součásti scénáře:

* **Serveru pro správu místní**: server pro správu hello spouští součásti Site Recovery:
  * **Konfigurační server**: koordinuje komunikaci a spravuje procesy data replikace a obnovení.
  * **Procesový server**: funguje jako replikační brána. Obdrží data z chráněných zdrojových počítačů; optimalizuje je pomocí ukládání do mezipaměti, komprese a šifrování; a odešle replikace dat tooAzure úložiště. Také obstará nabízenou instalaci počítače tooprotected služby Mobility a provádí automatického zjišťování virtuálních počítačů VMware.
  * **Hlavní cílový server**: Zpracovává replikační data během navracení služeb z Azure po obnovení.
    Můžete také nasadit server pro správu, který funguje pouze jako tooscale serveru proces nasazení.
* **Služba Mobility Hello**: Tato součást je nasazen na každý počítač (virtuálního počítače VMware nebo fyzických serverů), které chcete tooreplicate tooAzure. Se zaznamenává datové zápisy na počítači hello a předává je toohello procesový server.
* **Azure**: nepotřebujete toocreate všechny virtuální počítače Azure toohandle replikace a převzetí služeb při selhání. Hello služba Site Recovery zpracovává správu dat a data se replikují přímo tooAzure úložiště. Replikované virtuální počítače Azure se spouštějí automaticky jenom v případě, že dojde k převzetí služeb při selhání tooAzure. Ale pokud chcete toofail zpět z Azure toohello místní lokality, budete potřebovat tooset nahoru virtuálnímu počítači Azure tooact jako procesní server.

Následující obrázek (vytvořený Henry Robalino) Hello znázorňuje interakci tyto komponenty:

![Architektura součástí](./media/site-recovery-vmware-to-azure/v2a-architecture-henry.png)

## <a name="capacity-planning"></a>Plánování kapacity
Při plánování kapacity, zde je, co potřebujete toothink o:

* **Hello zdrojové prostředí**: plánování nebo hello VMware infrastruktury a zdrojový počítač požadavky na kapacitu.
* **server pro správu Hello**: plánování hello místní servery pro správu používající součásti Site Recovery.
* **Šířka pásma sítě ze zdroje tootarget**: plánování šířky pásma sítě vyžadovaná pro replikaci mezi zdrojem hello a Azure.

### <a name="source-environment-considerations"></a>Aspekty zdrojové prostředí
* **Maximální denní míry změny**: chráněného počítače lze použít pouze jeden procesový server. Jeden procesový server může zpracovat až too2 TB dat změnit za den. 2 TB tedy hello maximální denní data změnit rychlost, kterou je podporována pro chráněný počítač.
* **Maximální propustnost**: replikovaného počítače můžou patřit tooone účet úložiště v Azure. Standardní účet úložiště může zpracovat nesmí být delší než 20 000 požadavků za sekundu, a doporučujeme ponechat si hello počet IOPS napříč zdrojový počítač too20, 000. Například pokud je zdrojový počítač s 5 disky a každý disk generuje 120 (8 KB velikost) IOPS na zdroji hello, bude v rámci hello Azure za maximální IOPS disku 500. počet účtů úložiště vyžaduje Hello = source celkový počet IOPs/20 000.

### <a name="management-server-considerations"></a>Aspekty správy serveru
server pro správu Hello spustí součásti Site Recovery, které zpracovávají data optimalizace, replikaci a správu. By mělo být možné toohandle hello denní změnu míra kapacity mezi všechny úlohy spuštěné na chráněných počítačích a má dostatečná šířka pásma toocontinuously replikovat data tooAzure úložiště. Zejména:

* Hello procesový server přijímá data replikace z chráněného počítače a optimalizuje je pomocí ukládání do mezipaměti, komprese a šifrování před odesláním tooAzure. server pro správu Hello by měl mít dostatek prostředků tooperform tyto úlohy.
* procesový server Hello používá diskové mezipaměti. Doporučujeme disk samostatné mezipaměti 600 GB nebo více změn dat toohandle uložené v události hello přetížení sítě nebo výpadek. Během nasazení můžete nakonfigurovat mezipaměť hello v jakékoli jednotku, která má minimálně 5 GB dostupného úložiště, ale 600 GB je minimální doporučení hello.
* Jako osvědčený postup doporučujeme tímto serverem pro správu hello se nachází na hello stejné síti a segment sítě LAN jako hello počítače chcete tooprotect. Může být umístěn na jinou síť, ale budete chtít tooprotect by měl mít L3 sítě viditelnost tooit počítače.

Velikost doporučení pro server pro správu hello jsou shrnuty v následující tabulce hello:

| **Server pro správu procesoru** | **Paměť** | **Velikost disku mezipaměti** | **Míry změny dat** | **Chráněné počítače** |
| --- | --- | --- | --- | --- |
| 8 Vcpu (2 sockets * @ 2,5 GHz 4 jádra) |16 GB |300 GB |500 GB nebo méně |Nasazení serveru pro správu pomocí těchto nastavení tooreplicate méně než 100 počítačů. |
| 12 Vcpu (2 sockets * @ 2,5 GHz 6 jader) |18 GB |600 GB |500 GB too1 TB |Pomocí těchto nastavení tooreplicate 100 150 počítačů nasaďte server pro správu. |
| 16 Vcpu (2 sockets * @ 2,5 GHz 8 jader) |32 GB |1 TB |1 TB too2 TB |Pomocí těchto nastavení tooreplicate 150 až 200 počítačů nasaďte server pro správu. |
| Nasazení jiný procesový server | | |> 2 TB |Nasaďte další proces servery, pokud replikujete více než 200 počítačů, nebo pokud změny dat hello denní míra překročí 2 TB. |

Kde:

* Každý zdrojový počítač je nakonfigurovaný s 3 disky 100 GB.
* Použili jsme testu typovou úlohou úložiště 8 jednotky SAS 10 000 ot. / min s RAID 10 pro měření disku mezipaměti.

### <a name="network-bandwidth-from-source-tootarget"></a>Šířka pásma sítě ze zdroje tootarget
Zajistěte, aby vypočítat hello šířky pásma, která by byla zapotřebí pro počáteční replikaci a rozdílová replikace pomocí hello [nástroje Plánovač kapacity](site-recovery-capacity-planner.md).

#### <a name="throttling-bandwidth-used-for-replication"></a>Omezení šířky pásma používané pro replikaci
VMware provoz replikovat tooAzure projde konkrétní procesní server. Můžete omezit hello šířky pásma, která je k dispozici pro replikaci Site Recovery na tomto serveru následujícím způsobem:

1. Otevřete hello modul snap-in Microsoft Azure Backup konzoly MMC na serveru pro správu hlavní hello nebo na serveru pro správu spuštěna další zřízený proces servery. Ve výchozím nastavení je zástupce služby Microsoft Azure Backup vytvoří na ploše hello. Nebo ji můžete najít v C:\Program Files\Microsoft Azure Recovery Services Agent\bin\wabadmin.
2. V modulu snap-in hello, klikněte na **změnit vlastnosti**.

    ![Omezení šířky pásma změnit vlastnosti](./media/site-recovery-vmware-to-azure-classic/throttle1.png)
3. Na hello **omezování** zadejte hello šířku pásma, které lze použít pro replikaci Site Recovery a hello použít plánování.

    ![Omezení šířky pásma replikace](./media/site-recovery-vmware-to-azure-classic/throttle2.png)

Můžete také nastavit omezení pomocí prostředí PowerShell. Tady je příklad:

    Set-OBMachineSetting -WorkDay $mon, $tue -StartWorkHour "9:00:00" -EndWorkHour "18:00:00" -WorkHourBandwidth (512*1024) -NonWorkHourBandwidth (2048*1024)

#### <a name="maximizing-bandwidth-usage"></a>Tím se maximalizuje využití šířky pásma
Šířka pásma hello tooincrease využívaných Azure Site Recovery pro replikaci, je nutné toochange klíč registru.

Hello následující klíčové prvky hello počet vláken na replikaci disku, které se používají při replikaci:

    HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\Replication\UploadThreadsPerVM

 V síti zřízený musí tento klíč registru toobe se změnil z jeho výchozí hodnoty. Podporujeme nesmí být delší než 32.  

Další informace o plánování kapacity podrobné najdete v tématu [Plánovač kapacity Site Recovery](site-recovery-capacity-planner.md).

### <a name="additional-process-servers"></a>Další proces servery
Pokud potřebujete tooprotect více než 200 počítačů nebo vaše každodenní míru změn je větší, 2 TB, můžete přidat další servery toohandle hello zatížení. tooscale out, můžete postupovat následovně:

* Zvýšíte číslo hello serverů pro správu. Například můžete chránit až too400 počítačů se dvěma servery pro správu.
* Přidejte další proces servery a použít tyto toohandle provoz místo (nebo kromě) hello management server.

Tato tabulka popisuje scénář, ve kterém:

* Nastavit toouse hello původní správy serveru jako pouze konfigurační server.
* Nastavení serveru další proces.
* Nakonfigurujete chráněné virtuální počítače toouse hello další procesový server.
* Každý počítač chráněného zdroje je nakonfigurovaný s tři disky 100 GB.

| **Původní server pro správu**<br/><br/>(konfigurační server) | **Další procesového serveru** | **Velikost disku mezipaměti** | **Míry změny dat** | **Chráněné počítače** |
| --- | --- | --- | --- | --- |
| 8 Vcpu (2 sockets * 4 jádra @ 2,5 GHz), 16 GB paměti RAM |4 Vcpu (2 sockets * 2 jádra @ 2,5 GHz), 8 GB paměti RAM |300 GB |250 GB nebo méně |Můžete replikovat počítače 85 nebo méně. |
| 8 Vcpu (2 sockets * 4 jádra @ 2,5 GHz), 16 GB paměti RAM |8 Vcpu (2 sockets * 4 jádra @ 2,5 GHz), je 12 GB paměti RAM |600 GB |TB too1 250 GB |Můžete provádět replikaci 85 150 počítačů. |
| 12 Vcpu (2 sockets * @ 2,5 GHz 6 jader), 18 GB paměti RAM |12 Vcpu (2 sockets * @ 2,5 GHz 6 jader) 24 GB paměti RAM |1 TB |1 TB too2 TB |150 225 počítače můžete replikovat. |

Jak škálovat vaše servery závisí na vaši volbu pro model škálování nebo Škálováním na více systémů. Vertikální navýšení kapacity nasazením několik vyšší kategorie správy a proces servery nebo vertikální navýšení kapacity provádíte nasazení více serverů s méně prostředků. Například: Pokud potřebujete tooprotect 220 počítače, můžete provést jednu z následujících hello:

* Nakonfigurujte 12 Vcpu a 18 GB paměti RAM hello původního serveru pro správu. Konfigurace serveru další proces s 12 Vcpu a 24 GB paměti RAM. Nakonfigurujte chráněné počítače toouse pouze hello další procesový server.
* Nakonfigurujte dva servery pro správu (2 x 8 Vcpu, 16 GB paměti RAM) a dva další proces servery (1 × 8 Vcpu a 4vCPUs x 1 toohandle 135 + 85 (220) počítače). Konfigurace serverů pro další proces pouze hello systému chráněných počítačů toouse.

Postupujte podle pokynů hello v [nasadit další proces servery](#deploy-additional-process-servers) tooset nahoru Další procesový server.

## <a name="before-you-start-deployment"></a>Před zahájením nasazení
Hello následující tabulka představuje souhrn hello požadavky pro tento scénář nasazovat.

### <a name="azure-prerequisites"></a>Požadavky Azure
| **Požadavek** | **Podrobnosti** |
| --- | --- |
| **Účet Azure** |Potřebujete účet [Microsoft Azure](https://azure.microsoft.com/). Můžete začít s [bezplatnou zkušební verzí](https://azure.microsoft.com/pricing/free-trial/). Další informace o cenách za Site Recovery najdete v tématu [Site Recovery](https://azure.microsoft.com/pricing/details/site-recovery/). |
| **Úložiště Azure** |Je třeba data toostore replikovat účtu úložiště Azure. Replikovaná data jsou uložena v úložišti Azure a virtuálních počítačích Azure se spouštějí v případě převzetí služeb při selhání. <br/><br/>Potřebujete [účet se standardním geograficky redundantním úložištěm](../storage/storage-redundancy.md#geo-redundant-storage). Hello účet musí být ve stejné oblasti jako služba Site Recovery hello hello a možné přidružit k hello stejného předplatného. Účty úložiště toopremium replikace není aktuálně podporována a by se neměla používat.<br/><br/>Nepodporujeme přesunutí účty úložiště vytvořené pomocí hello [portál Azure](../storage/storage-create-storage-account.md) mezi skupinami prostředků. Další informace najdete v tématu [tooMicrosoft Úvod Azure Storage](../storage/storage-introduction.md).<br/><br/> |
| **Síť Azure** |Budete potřebovat virtuální síť Azure, virtuální počítače Azure připojí toowhen převzetí služeb při selhání. Hello virtuální síť Azure musí být v hello stejné oblasti jako trezor Site Recovery hello.<br/><br/>toofail zpět po převzetí služeb při selhání tooAzure, budete potřebovat síť VPN pro připojení (nebo Azure ExpressRoute) nastavit z hello Azure toohello místní síť. |

### <a name="on-premises-prerequisites"></a>Místní požadavky
| **Požadavek** | **Podrobnosti** |
| --- | --- |
| **Server pro správu** |Je nutné místnímu serveru Windows 2012 R2 spuštěné na virtuálním počítači nebo fyzickém serveru. Na tomto serveru pro správu jsou nainstalovány všechny součásti Site Recovery místní hello.<br/><br/> Doporučujeme že nasadit server hello jako vysoce dostupný virtuální počítač VMware. Místní lokalita toohello navrácení služeb po obnovení z Azure je vždy tooVMware virtuálních počítačů bez ohledu na to, jestli můžete převzít služby při selhání virtuálních počítačů nebo fyzických serverů. Pokud nenakonfigurujete server pro správu hello jako virtuální počítače VMware, musíte jako přenosem navrácení služeb po obnovení virtuálního počítače VMware tooreceive tooset samostatné hlavní cílový server.<br/><br/>Hello server by neměl být řadič domény.<br/><br/>Hello server musí mít statickou IP adresu.<br/><br/>název hostitele Hello hello serveru musí být maximálně 15 znaků nebo méně.<br/><br/>národní prostředí operačního systému Hello by měl být pouze angličtinu.<br/><br/>server pro správu Hello vyžaduje přístup k Internetu.<br/><br/>Potřebujete odchozí přístup ze serveru hello následujícím způsobem: dočasný přístup na HTTP 80 během instalace součásti Site Recovery hello (toodownload MySQL); probíhající odchozí přístup na protokol HTTPS 443 pro správu replikace; probíhající odchozí přístup na HTTPS 9443 pro provoz replikace (Tento port lze upravit).<br/><br/> Zkontrolujte, zda že jsou přístupné ze serveru pro správu hello tyto adresy URL: <br/>- \*.hypervrecoverymanager.windowsazure.com<br/>- \*.accesscontrol.windows.net<br/>- \*.backup.windowsazure.com<br/>- \*.blob.core.windows.net<br/>- \*.store.core.windows.net<br/>-https://www.msftncsi.com/ncsi.txt<br/>- [https://dev.MySQL.com/Get/Archives/MySQL-5.5/MySQL-5.5.37-Win32.msi] (https://dev.mysql.com/get/archives/mysql-5.5/mysql-5.5.37-win32.msi " https://dev.mysql.com/get/archives/mysql-5.5/mysql-5.5.37-win32.msi")<br/><br/>Pokud máte na serveru hello pravidla brány firewall založená na adresu IP, zkontrolujte, jestli hello pravidla umožňují komunikaci tooAzure. Je třeba tooallow hello [rozsahy IP Datacentra Azure](https://www.microsoft.com/download/details.aspx?id=41653) a hello port HTTPS (443). Musíte taky toowhitelist rozsahy IP adres pro hello oblast Azure svého předplatného a západní USA. Adresa URL Hello [https://dev.mysql.com/get/archives/mysql-5.5/mysql-5.5.37-win32.msi] (https://dev.mysql.com/get/archives/mysql-5.5/mysql-5.5.37-win32.msi " https://dev.mysql.com/get/archives/mysql-5.5/mysql-5.5.37-win32.msi") ke stahování MySQL. |
| **Hostitel VMware vCenter/ESXi** |Budete potřebovat jeden nebo více VMware vSphere ESX/ESXi hypervisory Správa virtuálních počítačů VMware ESX/ESXi verze 6.0, 5.5 nebo 5.1 s hello nejnovější aktualizace.<br/><br/> Doporučujeme nasadit VMware vCenter server toomanage hostitele ESXi. Měla by být spuštěna s vCenter verze 6.0 nebo 5.5 s nejnovějšími aktualizacemi hello.<br/><br/>Všimněte si, že Site Recovery nepodporuje nové vCenter a vSphere 6.0 funkce, jako křížové řešení vMotion vCenter, virtuální svazky a úložiště DRS. Podpora pro obnovení lokality je omezená toofeatures, které byly také dostupné ve verzi 5.5. |
| **Chráněné počítače** |**Azure**<br/><br/>Počítače, které chcete tooprotect musí být v souladu s [požadavky Azure](site-recovery-prereq.md) pro vytváření virtuálních počítačů Azure.<br><br/>Pokud chcete tooconnect toohello virtuální počítače Azure po převzetí služeb při selhání, je nutné tooenable připojení ke vzdálené ploše v místní bráně firewall hello.<br/><br/>Kapacita jednotlivých disků na chráněných počítačích by neměla být větší než 1 023 GB. Virtuální počítač může mít až too64 disků (tedy až too64 TB). Pokud máte disky, které jsou větší než 1 TB, zvažte použití replikace databáze, například SQL serveru Always On nebo Oracle Data Guard.<br/><br/>Minimální 2 GB volného místa na disku hello instalace pro instalaci součásti.<br/><br/>Sdílené hostované clustery disků nejsou podporované. Pokud máte skupinové nasazení, zvažte použití replikace databáze, například SQL serveru Always On nebo Oracle Data Guard.<br/><br/>Unified Extensible Firmware Interface (UEFI) / není podporované spouštění Extensible Firmware Interface.<br/><br/>Názvy počítačů musí obsahovat 1 až 63 znaků (písmena, číslice a pomlčky). Hello název musí začínat písmenem nebo číslicí a končit písmenem nebo číslicí. Jakmile je počítač je chráněný, můžete upravit hello název Azure.<br/><br/>**Virtuální počítače VMware**<br/><br>Je nutné tooinstall VMware vSphere PowerCLI 6.0. na serveru pro správu hello (konfigurační server).<br/><br/>Virtuální počítače VMware, budete ho chtít tooprotect by měl mít nástroje VMware nainstalovaná a spuštěná.<br/><br/>Pokud hello zdrojového virtuálního počítače má seskupování síťových adaptérů, převede se tooa po převzetí služeb při selhání tooAzure jeden síťový adaptér.<br/><br/>Pokud chráněných virtuálních počítačů iSCSI disk, Site Recovery převede hello chráněný disk iSCSI virtuálního počítače do souboru virtuálního pevného disku při hello virtuálních počítačů při selhání tooAzure. Pokud cíle iSCSI, dosažitelný z hello virtuálního počítače Azure, se připojí cílový tooiSCSI a v podstatě najdete v části dva disky: hello disku virtuálního pevného disku na disk iSCSI zdroj hello pro virtuální počítač Azure a hello. V takovém případě musíte toodisconnect hello cíle iSCSI, který se zobrazí na hello při selhání virtuálního počítače Azure.<br/><br/>Další informace o oprávnění uživatele VMware hello potřebuje obnovení lokality, najdete v části [VMware oprávnění pro přístup k systému vCenter](#vmware-permissions-for-vcenter-access).<br/><br/> **Počítače Windows serveru (u virtuálního počítače VMware nebo fyzický server)**<br/><br/>Hello server by měl být spuštěn podporovaný 64bitový operační systém: Windows Server 2012 R2, Windows Server 2012 nebo Windows Server 2008 R2 s v minimálně SP1.<br/><br/>Hello operačního systému by měly být nainstalovány na jednotce C a hello disk operačního systému musí být základní disk systému Windows. (hello operačního systému by se neměly instalovat na dynamický disk systému Windows.)<br/><br/>Pro počítače, Windows Server 2008 R2 musíte toohave rozhraní .NET Framework 3.5.1 nainstalována.<br/><br/>Je třeba tooprovide účtu správce (musí být místní správce na počítači Windows hello) pro hello nabízené instalace hello služba Mobility na serverech s Windows. Pokud hello předpokladu, že účet je jiný doménový účet, musíte toodisable řízení vzdáleného přístupu uživatele v místním počítači hello. Další informace najdete v tématu [instalaci služby mobility hello nabízenou instalaci](#install-the-mobility-service-with-push-installation).<br/><br/>Site Recovery podporuje virtuální počítače s diskem RDM. Během navrácení služeb po obnovení bude obnovení lokality používat hello RDM disk, pokud jsou k dispozici hello původního zdrojového virtuálního počítače a RDM disk. Pokud nejsou k dispozici, během navrácení služeb po obnovení, Site Recovery vytvoří nový soubor VMDK pro každý disk.<br/><br/>**Počítače se systémem Linux**<br/><br/>Je třeba podporovaný 64bitový operační systém: Red Hat Enterprise Linux 6.7; Centos verze 6.5, 6.6 nebo 6.7; Oracle Enterprise Linux 6.4 nebo 6.5 systémem hello Red Hat kompatibilní jádra nebo nedělitelné Enterprise jádra verze 3 (UEK3); SUSE Linux Enterprise Server 11 SP3.<br/><br/>soubory/etc/hosts na chráněných počítačích by měly obsahovat položky, které mapují hello místního hostitele název tooIP adres přidružených všechny síťové adaptéry. <br/><br/>Pokud chcete, aby tooconnect tooan virtuální počítač Azure s Linuxem po převzetí služeb při selhání pomocí klienta Secure Shell (ssh), ujistěte se, že služba Secure Shell hello na hello chráněný počítač je nastavená toostart automaticky při spuštění systému a že povolit pravidla brány firewall ssh tooit připojení.<br/><br/>Ochrany lze povolit pouze pro počítače se systémem Linux s hello následující úložiště: souboru systému (EXT3, ETX4, ReiserFS, XFS); Funkce Multipath softwaru zařízení Mapper (multipath); Správce svazků (LVM2). Fyzické servery s HP CCISS řadič úložiště nejsou podporovány. systém souborů ReiserFS Hello je podporována pouze na SUSE Linux Enterprise Server 11 SP3.<br/><br/>Site Recovery podporuje virtuální počítače s diskem RDM. Během navrácení služeb po obnovení pro Linux Site Recovery nemá opakovaně hello RDM disk. Místo toho vytvoří nový soubor VMDK pro každý odpovídající RDM disk. |

Pouze pro virtuální počítač s Linuxem: Ujistěte se, že jste nastavení hello disk.enableUUID=true v hello konfigurační parametry hello virtuálních počítačů v prostředí VMware. Pokud tento řádek neexistuje, přidejte ji. Toto je požadovaná tooprovide konzistentní toohello UUID VMDK tak, aby ji připojí správně. Bez tohoto nastavení způsobí navrácení služeb po obnovení úplné stažení i v případě, že je k dispozici místní hello virtuálních počítačů. Přidání toto nastavení zajistí, že jsou přenášeny jenom rozdílové změny zpět během navrácení služeb po obnovení.

## <a name="step-1-create-a-vault"></a>Krok 1: Vytvoření trezoru
1. Přihlaste se toohello [portál Azure](https://manage.windowsazure.com/).
2. Rozbalte položku **datové služby** > **služeb zotavení** a klikněte na tlačítko **trezor Site Recovery**.
3. Klikněte na **Vytvořit nový** > **Rychlé vytvoření**.
4. V **název**, zadejte popisný název tooidentify hello trezoru.
5. V **oblast**, hello vyberte zeměpisnou oblast trezoru hello. toocheck podporované oblasti, najdete v části [podrobnosti o cenách Azure Site Recovery](https://azure.microsoft.com/pricing/details/site-recovery/).
6. Klikněte na **Vytvořit trezor**.
    ![Vytvoření trezoru](./media/site-recovery-vmware-to-azure-classic/quick-start-create-vault.png)

Zkontrolujte, zda tooconfirm hello stav panelu, který hello trezor byl úspěšně vytvořen. Hello trezor bude uvedený jako **Active** na hello hlavní **služeb zotavení** stránky.

## <a name="step-2-set-up-an-azure-network"></a>Krok 2: Nastavení sítě Azure
Nastavte síť Azure, aby virtuální počítače Azure budou připojené tooa sítě po převzetí služeb při selhání, a tak, aby navrácení služeb po obnovení toohello místní lokality může fungovat podle očekávání.

1. V hello portálu Azure, vyberte **vytvořit virtuální síť** a zadejte název sítě hello, rozsah IP adres a název podsítě.
2. Pokud potřebujete toodo navrácení služeb po obnovení, přidejte síť VPN nebo ExpressRoute toohello. Sítě VPN nebo ExpressRoute lze přidat toohello sítě i po převzetí služeb při selhání.

Další informace o sítě Azure najdete v tématu [Přehled virtuálních sítí](../virtual-network/virtual-networks-overview.md).

> [!NOTE]
> [Migrace sítí](../azure-resource-manager/resource-group-move-resources.md) napříč prostředků skupiny v hello stejného předplatného, nebo ve předplatných není podporována pro sítě používá pro nasazení Site Recovery.
>
>

## <a name="step-3-install-hello-vmware-components"></a>Krok 3: Instalace komponent VMware hello
Pokud chcete virtuální počítače VMware tooreplicate, postupujte podle těchto kroků na serveru pro správu hello:

1. [Stáhněte si](https://my.vmware.com/web/vmware/details?productId=491&downloadGroup=PCLI600R1) a nainstalujte VMware vSphere PowerCLI 6.0.
2. Restartujte hello server.

## <a name="step-4-download-a-vault-registration-key"></a>Krok 4: Stáhněte si registrační klíč trezoru
1. Ze serveru pro správu hello otevřete konzolu hello Site Recovery v Azure. Na hello **služeb zotavení** klikněte na tlačítko hello trezoru tooopen hello **rychlý Start** stránky. Můžete také otevřít hello **rychlý Start** kdykoli kliknutím na ikonu hello.

    ![Ikona rychlý Start](./media/site-recovery-vmware-to-azure-classic/quick-start-icon.png)
2. Na hello **rychlý Start** klikněte na tlačítko **Příprava cílové prostředky** > **stáhněte si registrační klíč**. soubor registračního Hello je generován automaticky. Je platná pět dní po jeho vygenerování.

## <a name="step-5-install-hello-management-server"></a>Krok 5: Instalace serveru pro správu hello
> [!TIP]
> Zkontrolujte, zda že jsou přístupné ze serveru pro správu hello tyto adresy URL:
>
> * *.hypervrecoverymanager.windowsazure.com
> * *.accesscontrol.windows.net
> * *.backup.windowsazure.com
> * *.blob.core.windows.net
> * *.store.core.windows.net
> * https://dev.mysql.com/get/archives/mysql-5.5/mysql-5.5.37-win32.msi
> * https://www.msftncsi.com/ncsi.txt
>
>



>[!VIDEO https://channel9.msdn.com/Blogs/Azure/Enhanced-VMware-to-Azure-Setup-Registration/player]



1. Na hello **rychlý Start** stránky, stáhněte si hello jednotnou instalaci souboru toohello serveru.
2. Spusťte instalační program toostart hello instalační soubor v hello **Unified instalace nástroje Site Recovery** průvodce.
3. V **před zahájením**, vyberte **nainstalovat hello konfigurační server a procesový server**.

   ![Než začnete](./media/site-recovery-vmware-to-azure-classic/combined-wiz1.png)
4. V **licence k softwaru třetích stran**, klikněte na tlačítko **souhlasím** toodownload a nainstalujte MySQL.

    ![Software jiných výrobců](./media/site-recovery-vmware-to-azure-classic/combined-wiz105.PNG)
5. V **registrace**, procházet a vyberte hello registrační klíč, který jste stáhli z trezoru hello.

    ![Registrace](./media/site-recovery-vmware-to-azure-classic/combined-wiz3.png)
6. V **nastavení Internetu**, zadejte, jak hello zprostředkovatel, který běží na konfiguračním serveru hello se budou připojovat tooAzure Site Recovery přes hello Internet.

   * Pokud chcete tooconnect s hello proxy, který je aktuálně nastavený na hello počítače, vyberte **připojit se s existujícím nastavením proxy serveru**.
   * Pokud chcete hello zprostředkovatele tooconnect přímo, vyberte **připojit přímo bez proxy**.
   * Pokud hello stávající proxy server vyžaduje ověřování, nebo pokud chcete použít pro připojení poskytovatele hello toouse vlastní proxy server, vyberte **Connect s vlastním nastavením proxy**.
     * Pokud používáte vlastní proxy server, budete potřebovat toospecify hello adresu, port a přihlašovací údaje.
     * Pokud používáte proxy server, které by už mít povolené hello následující adresy URL:
       * *.hypervrecoverymanager.windowsazure.com    
       * *.accesscontrol.windows.net
       * *.backup.windowsazure.com
       * *.blob.core.windows.net
       * *.store.core.windows.net

    ![Brána firewall](./media/site-recovery-vmware-to-azure-classic/combined-wiz4.png)

1. V **Kontrola předpokladů**, instalace se spustí kontrola toomake jistotu, že můžete spustit instalaci hello.

    ![Požadavky](./media/site-recovery-vmware-to-azure-classic/combined-wiz5.png)

     Pokud se zobrazí upozornění o hello **čas globální synchronizace kontrola**, ověřte, že hello času na hello systémových hodin (**datum a čas** nastavení) hello stejné jako hello časové pásmo.

     ![Čas synchronizace problém](./media/site-recovery-vmware-to-azure-classic/time-sync-issue.png)

1. V **MySQL konfigurace**, vytvořit přihlašovací údaje pro přihlášení instance toohello MySQL serveru, který bude nainstalován.

    ![MySQL](./media/site-recovery-vmware-to-azure-classic/combined-wiz6.png)
2. V **prostředí podrobnosti**vyberte, jestli budete virtuální počítače VMware tooreplicate. Pokud jste, instalační program kontroluje, že je nainstalována PowerCLI 6.0.

    ![MySQL](./media/site-recovery-vmware-to-azure-classic/combined-wiz7.png)
3. V **umístění instalovat**, vyberte, kde chcete tooinstall hello binární soubory a úložiště mezipaměti hello. Můžete vybrat jednotku, která má minimálně 5 GB volného místa na disku, ale doporučujeme jednotky mezipaměti s alespoň 600 GB volného místa na disku.

   ![Umístění instalace](./media/site-recovery-vmware-to-azure-classic/combined-wiz8.png)
4. V **výběr sítě**, zadejte hello naslouchací proces (síťový adaptér a SSL port) na které hello konfigurační server bude odesílat a přijímat data replikace. Můžete upravit hello výchozí port (9443). Port 443 se v přidání toothis port, použije webový server, které orchestrují operací replikace. Nepoužívejte 443 pro příjem přenosů replikace.

    ![Výběr sítě](./media/site-recovery-vmware-to-azure-classic/combined-wiz9.png)



1. V **Souhrn**, zkontrolujte hello informace a klikněte na tlačítko **nainstalovat**. Po dokončení instalace se vygeneruje heslo. To budete potřebovat při povolení replikace, tak ho zkopírujte a uložte ho na bezpečném místě.

   ![Souhrn](./media/site-recovery-vmware-to-azure-classic/combined-wiz10.png)


> [!WARNING]
> musíte nastavit Hello proxy agenta služeb zotavení Microsoft Azure.
> Po dokončení instalace hello spusťte hello Microsoft Azure Recovery Services prostředí z nabídky Windows Start hello. V příkazovém okně hello, které se otevře spusťte následující sadu příkazů tooset nastavení serveru proxy hello hello.
>
>
    $pwd = ConvertTo-SecureString -String ProxyUserPassword
    Set-OBMachineSetting -ProxyServer http://myproxyserver.domain.com -ProxyPort PortNumb – ProxyUserName domain\username -ProxyPassword $pwd
    net stop obengine
    net start obengine
>


### <a name="run-setup-from-hello-command-line"></a>Spusťte instalační program z příkazového řádku hello
Také Průvodce můžete spustit hello jednotná z příkazového řádku hello, následujícím způsobem:

    UnifiedSetup.exe [/ServerMode <CS/PS>] [/InstallDrive <DriveLetter>] [/MySQLCredsFilePath <MySQL credentials file path>] [/VaultCredsFilePath <Vault credentials file path>] [/EnvType <VMWare/NonVMWare>] [/PSIP <IP address toobe used for data transfer] [/CSIP <IP address of CS toobe registered with>] [/PassphraseFilePath <Passphrase file path>]

Kde:

* /ServerMode: Povinné. Určuje, jestli by měl hello instalace instalovat hello proces a konfigurace serverů nebo hello proces pouze server (servery použité tooinstall další proces). Vstupní hodnoty: CS, PS.
* InstallDrive: povinné. Určuje složku hello kterém jsou nainstalované komponenty hello.
* / MySQLCredFilePath. Povinné. Určuje hello cestě tooa souboru, kde jsou uložené přihlašovací údaje serveru hello MySQL. Získáte soubor hello toocreate hello šablony.
* / VaultCredFilePath. Povinné. Umístění souboru s přihlašovacími údaji hello.
* /EnvType: Povinné. Typ instalace. Hodnoty: VMware, NonVMware.
* /PSIP a /CSIP: Povinné. IP adresa hello proces a konfigurace serveru.
* /PassphraseFilePath: Povinné. Umístění souboru hello přístupové heslo.
* / ByPassProxy. Volitelné. Určuje hello management server, který se připojuje tooAzure bez serveru proxy.
* /ProxySettingsFilePath: Volitelné. Určuje nastavení pro vlastní proxy server (výchozí proxy server na hello serveru, který vyžaduje ověření) nebo vlastní proxy server.

## <a name="step-6-set-up-credentials-for-hello-vcenter-server"></a>Krok 6: Nastavení pověření pro hello vCenter server
> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Enhanced-VMware-to-Azure-Discovery/player]
>
>

procesový server Hello může automaticky zjistit virtuální počítače VMware, která se spravují přes vCenter server. Site Recovery pro automatické zjišťování, potřebuje účet a přihlašovací údaje, které můžete přístup k serveru vCenter hello. Tato akce není relevantní, pokud replikujete jenom fyzických serverů.

účet hello tooset a přihlašovací údaje:

1. Na serveru vCenter hello vytvořte roli (**Azure_Site_Recovery**) na úrovni vCenter hello s hello [požadovaná oprávnění](#vmware-permissions-for-vcenter-access).
2. Přiřadit hello **Azure_Site_Recovery** role tooa vCenter uživatele.

   > [!NOTE]
   > VCenter uživatelský účet, který má roli hello jen pro čtení můžete spustit převzetí služeb při selhání, aniž by předtím vypnul chráněných zdrojových počítačů. Pokud chcete tooshut dolů těch počítačů, budete potřebovat hello Azure_Site_Recovery role. Pokud máte pouze migrace virtuálních počítačů z VMware tooAzure a nepotřebujete toofail zpět, je dostatečná role hello jen pro čtení.
   >
   >
3. tooadd hello účtu, otevřete **cspsconfigtool**. Je k dispozici jako zástupce na ploše hello a umístěné ve složce \home\svsystems\bin hello [umístění INSTALOVAT].
4. Na hello **Správa účtů** , klikněte na **přidat účet**.

    ![Přidat účet](./media/site-recovery-vmware-to-azure-classic/credentials1.png)
5. V **Podrobnosti účtu**, přidat přihlašovací údaje, které se dají použít tooaccess hello vCenter server. Může trvat déle než 15 minut pro tooappear název účtu hello hello portálu. okamžitě, klikněte na tlačítko tooupdate **aktualizovat** na hello **konfigurační servery** kartě.

    ![Podrobnosti](./media/site-recovery-vmware-to-azure-classic/credentials2.png)

## <a name="step-7-add-vcenter-servers-and-esxi-hosts"></a>Krok 7: Přidejte servery vCenter server a hostitelů ESXi
Pokud replikujete virtuální počítače VMware, potřebujete tooadd vCenter server (nebo hostiteli ESXi).

1. Na hello **servery** > **konfigurační servery** vyberte **přidat server vCenter**.

    ![vCenter](./media/site-recovery-vmware-to-azure-classic/add-vcenter1.png)
2. Přidejte hello vCenter server nebo podrobnosti o hostiteli ESXi, hello název účtu hello, kterou jste zadali, že tooaccess hello vCenter server v předchozím kroku hello a hello procesového serveru, které budou virtuální počítače VMware použité toodiscover, která jsou spravována serverem vCenter hello. Hello vCenter server nebo hostiteli ESXi musí nacházet ve stejné sítě jako hello server, na které hello je nainstalován server proces hello.

   > [!NOTE]
   > Pokud přidáváte hello vCenter server nebo hostiteli ESXi pomocí účtu, který nemá oprávnění správce na serveru vCenter nebo hostitelů hello, ujistěte se, hello vCenter nebo účty ESXi mít tato oprávnění povoleno: Datacenter, úložiště, složku a Jost, sítě, Prostředek, virtuální počítač a vSphere distribuované přepínače. Hello vCenter server potřebuje oprávnění povolená zobrazení hello úložiště.
   >
   >

    ![Přidat vCenter server](./media/site-recovery-vmware-to-azure-classic/add-vcenter2.png)
3. Po dokončení zjišťování serveru vCenter hello bude uvedený na hello **konfigurační servery** kartě.

    ![vCenter](./media/site-recovery-vmware-to-azure-classic/add-vcenter3.png)

## <a name="step-8-create-a-protection-group"></a>Krok 8: Vytvoření skupiny ochrany
> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Enhanced-VMware-to-Azure-Protection/player]
>
>

Skupiny ochrany jsou logickými seskupeními virtuálních počítačů nebo fyzických serverů, které má tooprotect pomocí hello stejné nastavení ochrany. Použít skupinu ochrany tooa nastavení ochrany, a tato nastavení jsou použité tooall počítače (virtuální nebo fyzická) přidat toohello skupiny.

1. Přejděte příliš**chráněné položky** > **skupiny ochrany** a klikněte na ikonu tooadd hello skupiny ochrany.

    ![Vytvořit skupinu ochrany](./media/site-recovery-vmware-to-azure-classic/protection-groups1.png)
2. Na hello **zadejte nastavení skupiny ochrany** stránky, zadejte název pro skupinu hello. V hello **z** rozevíracího seznamu, vyberte hello konfigurační server, na kterém chcete toocreate hello skupiny. **Cíl** je Microsoft Azure.

    ![Nastavení skupiny ochrany](./media/site-recovery-vmware-to-azure-classic/protection-groups2.png)
3. Na hello **zadejte nastavení replikace** nakonfigurujte nastavení hello replikace, která se použije pro všechny počítače hello ve skupině hello.

    ![Skupiny ochrany replikace](./media/site-recovery-vmware-to-azure-classic/protection-groups3.png)

   * **Konzistence pro víc Virtuálních**: Pokud tuto možnost zapnout, se vytváří body obnovení sdílené konzistentní s aplikací mezi hello počítačů ve skupině ochrany hello. Toto nastavení je nejdůležitější, když všechny počítače ve skupině ochrany hello běží stejné zatížení hello. Všechny počítače budou obnovené toohello stejného datového bodu. Toto je k dispozici, jestli replikujete virtuální počítače VMware nebo fyzických serverů (Windows nebo Linux).
   * **Prahová hodnota RPO**: Nastaví hello plánovaný bod obnovení. Výstrahy jsou generovány, pokud hello replikace průběžné ochrany dat překročí prahovou hodnotu RPO hello nakonfigurované.
   * **Uchování bodu obnovení**: Určuje interval pro uchovávání hello. Chráněné počítače může být obnovena tooany bodu v rámci tohoto okna.
   * **Frekvence snímkování konzistentní aplikace vzhledem**: Určuje, jak často budou vytvořeny body obnovení, obsahující snímky konzistentní s aplikacemi.

Když vyberete hello zaškrtnutí, skupina ochrany vytvořena s hello název, který jste zadali. Kromě toho se vytvoří druhé skupiny ochrany s názvem hello *název skupiny ochrany*- navrácení služeb po obnovení. Pokud po převzetí služeb při selhání tooAzure selhat toohello zpět na místní lokalitu, použije se tato skupina ochrany. Skupiny ochrany hello můžete sledovat, jak jste vytvořili na hello **chráněné položky** stránky.

## <a name="step-9-install-hello-mobility-service"></a>Krok 9: Instalace služby Mobility hello
Hello prvním krokem při povolování ochrany pro virtuální počítače a fyzické servery je služba Mobility tooinstall hello. Můžete provést dvěma způsoby:

* Automaticky push a nainstalujte na každém počítači ze serveru proces hello hello služby. Když přidáte skupinu ochrany tooa počítače a již používáte příslušnou verzi hello služby Mobility, nedojde, nabízenou instalaci. Služba hello také automaticky nainstalujete enterprise nabízené způsobem, jako je WSUS nebo System Center Configuration Manager. Ujistěte se, že jste nastavili server pro správu hello tím.
* Nainstalujte službu hello ručně na každém počítači, které chcete tooprotect. Ujistěte se, že jste nastavili server pro správu hello tím.

### <a name="install-hello-mobility-service-with-push-installation"></a>Instalaci služby Mobility hello nabízenou instalaci
Přidáte-li skupinu ochrany tooa počítače, hello služba Mobility je automaticky instaluje a nainstalovat na každém počítači s hello procesový server.

#### <a name="prepare-for-automatic-push-on-windows-machines"></a>Příprava pro automatické nabízené oznámení na počítačích s Windows
počítače s Windows tooprepare, který hello služby Mobility můžete automaticky nainstalovat hello procesového serveru:

1. Vytvoření účtu, že tento server hello procesu můžete použít počítač tooaccess hello. Hello účet by měl mít oprávnění správce (místní nebo doménový). Tyto přihlašovací údaje se používají pouze pro nabízenou instalaci služby Mobility hello.

   > [!NOTE]
   > Pokud nepoužíváte účet domény, budete potřebovat toodisable řízení vzdáleného přístupu uživatele v místním počítači hello. toodo, v registru hello pod HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System, přidejte položku DWORD hello LocalAccountTokenFilterPolicy s hodnotou 1 v části. položky registru hello tooadd z rozhraní příkazového řádku spusťte příkaz nebo pomocí prostředí PowerShell, zadejte  **`REG ADD HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System /v LocalAccountTokenFilterPolicy /t REG_DWORD /d 1`** .
   >
   >
2. Pro bránu Windows Firewall v počítači hello, které chcete tooprotect, vyberte **povolit aplikace nebo funkci průchod bránou Firewall**a povolte **sdílení souborů a tiskáren** a **Správa systému Windows Instrumentace**. Pro počítače, které patří tooa domény můžete nakonfigurovat zásady brány firewall hello se objekt zásad skupiny.

   ![Nastavení brány firewall](./media/site-recovery-vmware-to-azure-classic/mobility1.png)
3. Přidejte hello účtu, který jste vytvořili:

   * Otevřete nástroj **cspsconfigtool**. Je k dispozici jako zástupce na ploše hello a umístěné ve složce \home\svsystems\bin hello [umístění INSTALOVAT].
   * Na hello **Správa účtů** , klikněte na **přidat účet**.
   * Přidejte hello účet, který jste vytvořili. Po přidání účtu hello, budete potřebovat přihlašovací údaje hello tooprovide při přidání skupiny ochrany tooa počítače.

#### <a name="prepare-for-automatic-push-on-linux-servers"></a>Příprava pro automatické nabízené oznámení na serverech s Linuxem
1. Ujistěte se, že počítač Linux hello tooprotect je podporována, jak je popsáno v [místní požadavky](#on-premises-prerequisites). Zkontrolujte, zda je připojení k síti mezi počítači hello chcete tooprotect a hello management server, který spouští hello procesový server.
2. Vytvoření účtu, že tento server hello procesu můžete použít počítač tooaccess hello. Hello účet by měl být kořenový uživatel na zdrojovém serveru se Linux hello. Tyto přihlašovací údaje se používají pouze pro nabízenou instalaci služby Mobility hello.

   * Otevřete nástroj **cspsconfigtool**. Je k dispozici jako zástupce na ploše hello a umístěné ve složce \home\svsystems\bin hello [umístění INSTALOVAT].
   * Na hello **Správa účtů** , klikněte na **přidat účet**.
   * Přidejte hello účet, který jste vytvořili. Po přidání účtu hello, budete potřebovat přihlašovací údaje hello tooprovide při přidání skupiny ochrany tooa počítače.
3. Zkontrolujte tento soubor/etc/hosts hello na zdroji hello Linux server obsahuje položky, které mapují hello názvem místního hostitele tooIP adresy přidružené k všechny síťové adaptéry.
4. Nainstalujte nejnovější openssh, openssh server a openssl balíčky hello na hello počítače, které chcete tooprotect.
5. Ujistěte se, že SSH je povolené a spuštěné na port 22.
6. Povolte ověřování pomocí protokolu SFTP subsystém a heslo v souboru sshd_config hello následujícím způsobem:

   * Přihlaste se jako kořenový adresář.
   * V souboru /etc/ssh/sshd_config hello najdete hello řádek, který začíná PasswordAuthentication.
   * Zrušením komentáře u hello řádku a změňte hodnotu hello z **žádné** příliš**Ano**.
   * Najít hello řádek, který začíná **subsystému** a zrušte komentář u hello řádku.

     ![Linux přepsat výchozí žádné subsystémů](./media/site-recovery-vmware-to-azure-classic/mobility2.png)

### <a name="install-hello-mobility-service-manually"></a>Ruční instalace služby Mobility hello
Instalační programy Hello jsou k dispozici v C:\Program Files (x86) \Microsoft Azure Site Recovery\home\svsystems\pushinstallsvc\repository.

| Zdrojový operační systém | Instalační soubor služby Mobility |
| --- | --- |
| Windows Server (pouze 64bitová verze) |Microsoft-ASR_UA_9*.0.0_Windows_* release.exe |
| CentOS 6.4, 6.5, 6.6 (pouze 64bitové verze) |Microsoft-ASR_UA_9*.0.0_RHEL6-64_*release.tar.gz |
| SUSE Linux Enterprise Server 11 SP3 (pouze 64bitová verze) |Microsoft-ASR_UA_9*.0.0_SLES11-SP3-64_*release.tar.gz |
| Oracle Enterprise Linux 6.4, 6.5 (pouze 64bitová verze) |Microsoft-ASR_UA_9*.0.0_OL6-64_*release.tar.gz |

#### <a name="install-hello-mobility-service-manually-on-a-windows-server"></a>Instalaci služby Mobility hello ručně v systému Windows server
1. Stáhněte a spusťte instalační program relevantní hello.
2. V části **Než začnete** vyberte **Služba Mobility**.

    ![Instalace služby mobility](./media/site-recovery-vmware-to-azure-classic/mobility3.png)
3. V **podrobnosti o konfiguraci serveru**, zadejte IP adresu hello hello serveru pro správu a hello přístupové heslo, který byl vygenerován při instalaci součásti serveru pro správu hello. Přístupové heslo hello můžete načíst tak, že spustíte  **<SiteRecoveryInstallationFolder>\home\sysystems\bin\genpassphrase.exe – n** na serveru pro správu hello.

    ![Služba Mobility](./media/site-recovery-vmware-to-azure-classic/mobility6.png)
4. V **umístění instalovat**, zachovat hello výchozí umístění a klikněte na **Další** toobegin instalace.
5. V **průběh instalace**, zkontrolujte instalaci a restartujte počítač hello, pokud se zobrazí výzva.

Můžete taky nainstalovat tak, že zadáte hello následující text hello příkazového řádku:

    UnifiedAgent.exe [/Role <Agent/MasterTarget>] [/InstallLocation <Installation Directory>] [/CSIP <IP address of CS toobe registered with>] [/PassphraseFilePath <Passphrase file path>] [/LogFilePath <Log File Path>]

V předchozím příkazu hello:

* / Role: povinné. Určuje, zda by měly být nainstalovány hello služba Mobility.
* / InstallLocation: povinné. Určuje, kde tooinstall hello služby.
* / PassphraseFilePath: povinné. Určuje heslo konfiguračního serveru hello.
* / LogFilePath: povinné. Určuje umístění souboru instalačního programu hello protokolu.

#### <a name="uninstall-hello-mobility-service-manually"></a>Ručně odinstalujte službu Mobility hello
Služba Mobility hello můžete odinstalovat pomocí **odinstalovat nebo změnit program** v Ovládacích panelech nebo pomocí příkazového řádku hello.

příkaz toouninstall Hello služba Mobility pomocí příkazového řádku hello je:

    MsiExec.exe /qn /x {275197FC-14FD-4560-A5EB-38217F80CBD1}

#### <a name="change-hello-ip-address-of-hello-management-server"></a>Změna hello IP adresy serveru pro správu hello
Po spuštění Průvodce hello, můžete změnit hello IP adresa serveru pro správu hello následujícím způsobem:

1. Otevřete hostconfig.exe souboru hello (nachází se na ploše hello).
2. Na hello **globální** změňte hello IP adresa serveru pro správu hello.

   > [!NOTE]
   > Změňte pouze hello IP adresa serveru pro správu hello. Hello číslo portu pro komunikaci se serverem pro správu musí být 443, a **použití HTTPS** musí být vlevo povolena. Neměňte heslo hello.
   >
   >

    ![IP adresa serveru pro správu](./media/site-recovery-vmware-to-azure-classic/host-config.png)

#### <a name="install-hello-mobility-service-manually-on-a-linux-server"></a>Instalaci služby Mobility hello ručně na Linux server
1. Zkopírujte hello odpovídající vkládání archivu toohello Linux počítače, které chcete tooprotect. Viz tabulka hello pod [ruční instalace služby Mobility hello](#install-the-mobility-service-manually) toodetermine které vkládání archivaci, můžete využít.
2. Otevřete program prostředí a extrahovat hello komprimované vkládání archivu tooa místní cestu spuštěním:`tar -xvzf Microsoft-ASR_UA_8.5.0.0*`
3. Vytvořte soubor s názvem passphrase.txt do místního adresáře toowhich hello jste extrahovali obsah hello hello vkládání archivu. toodo se kopie hello přístupové heslo z C:\ProgramData\Microsoft Azure lokality Recovery\private\connection.passphrase na serveru pro správu hello a uložte ho do passphrase.txt spuštěním  *`echo <passphrase> >passphrase.txt`*  v prostředí hello.
4. Instalaci služby Mobility hello zadáním hello následující příkaz:*`sudo ./install -t both -a host -R Agent -d /usr/local/ASR -i <IP address> -p <port> -s y -c https -P passphrase.txt`*
5. Zadejte hello interní IP adresu serveru pro správu hello a ujistěte se, že je vybrán port 443.

#### <a name="install-hello-mobility-service-from-hello-command-line"></a>Instalaci služby Mobility hello z příkazového řádku hello

Kopírovat heslo hello z C:\Program Files (x86) \InMage Systems\private\connection na serveru pro správu hello a uložte ho jako "passphrase.txt" na serveru pro správu hello. Spusťte následující příkazy hello. V našem příkladu hello adresa IP serveru pro správu je 104.40.75.37 a hello HTTPS port je 443:

tooinstall na provozním serveru:

    ./install -t both -a host -R Agent -d /usr/local/ASR -i 104.40.75.37 -p 443 -s y -c https -P passphrase.txt

tooinstall na hlavní cílový server hello:

    ./install -t both -a host -R MasterTarget -d /usr/local/ASR -i 104.40.75.37 -p 443 -s y -c https -P passphrase.txt


## <a name="step-10-enable-protection-for-a-machine"></a>Krok 10: Povolení ochrany pro počítač
tooenable ochrany, přidejte virtuální počítače a skupiny ochrany tooa fyzických serverů. Než začnete, Všimněte si následujících hello, pokud chráníte virtuální počítače VMware:

* Virtuální počítače VMware jsou zjištěny každých 15 minut, a může trvat déle než 15 minut pro ně tooappear hello portálu Site Recovery po zjišťování.
* Změny prostředí na virtuálním počítači hello (jako je třeba instalace nástroje VMware) může trvat víc než 15 minut toobe aktualizovat ve službě Site Recovery.
* Můžete zkontrolovat hello poslední zjištěné čas pro virtuální počítače VMware v hello **poslední obraťte se na** pole pro hello vCenter server nebo hostiteli ESXi, na hello **konfigurační servery** kartě.
* Pokud přidáte vCenter Server nebo hostiteli ESXi po vytvoření skupiny ochrany, může trvat déle než 15 minut pro toorefresh portálu Azure Site Recovery hello a toobe virtuálních počítačů, které jsou uvedené v hello **skupiny ochrany přidat počítače tooa**dialogové okno.
* Pokud chcete tooproceed okamžitě a přidejte skupinu ochrany tooa počítače bez čekání na hello naplánovaného zjišťování, zvýrazněte hello konfigurační server (nemáte klikněte na něj) a klikněte na tlačítko **aktualizovat**.

Navíc platí:

* Doporučujeme, abyste architektury skupin ochrany, aby odpovídaly vašich úloh. Například přidejte počítače spouštěné konkrétní aplikaci toohello stejné skupiny.
* Při přidání skupiny ochrany počítače tooa hello procesový server automaticky nabízených oznámení a nainstaluje služba Mobility hello, pokud již není nainstalována. Budete potřebovat toohave hello nabízené mechanismus připravené, jak je popsáno v předchozím kroku hello.

### <a name="add-machines-tooa-protection-group"></a>Přidat skupinu ochrany tooa počítače

1. Přejděte příliš**chráněné položky** > **skupiny ochrany** > **počítače** > **přidat počítače** .
2. Pokud chráníte virtuální počítače VMware v **vyberte virtuální počítače**, vyberte server vCenter, který spravuje virtuální počítače (nebo hello EXSi hostitele, na kterém používáte systém) a pak vyberte hello počítače.

    ![Povolit ochranu pro virtuální počítače](./media/site-recovery-vmware-to-azure-classic/enable-protection2.png)
3. Pokud chráníte fyzické servery, v **vyberte virtuální počítače**, otevřete hello **přidání fyzických počítačů** průvodce a zadejte popisný název a IP adresu hello. Potom vyberte operační systém řady hello.

   ![Povolte ochranu pro fyzické servery](./media/site-recovery-vmware-to-azure-classic/enable-protection1.png)
4. V **zadat cílové prostředky**, vyberte účet úložiště hello, který používáte pro replikaci a vyberte, zda text hello nastavení se použije pro všechny úlohy. Prémiové účty úložiště nejsou aktuálně podporovány.

   > [!NOTE]
   > Nepodporujeme přesunutí účty úložiště vytvořené pomocí hello [portál Azure](../storage/storage-create-storage-account.md) mezi skupinami prostředků.                           
   > [Migrace účtů úložiště](../azure-resource-manager/resource-group-move-resources.md) napříč prostředků skupiny v hello stejného předplatného, nebo ve předplatných není podporována pro účty úložiště používá pro nasazení Site Recovery.
   >
   >

    ![Konfigurace nastavení cíle](./media/site-recovery-vmware-to-azure-classic/enable-protection3.png)
5. V **zadejte účty**, vyberte hello účtu, který jste [nastavit](#install-the-mobility-service-with-push-installation) toouse pro automatickou instalaci hello služba Mobility.

    ![Zadejte účty](./media/site-recovery-vmware-to-azure-classic/enable-protection4.png)
6. Klikněte na tlačítko toofinish zaškrtnutí hello přidání počítače toohello ochranu skupiny a toostart počáteční replikace pro každý počítač.

   > [!NOTE]
   > Pokud byl připraven nabízenou instalaci, hello služba Mobility se automaticky nainstaluje na počítače, které ještě nemají ho, když jste přidávány toohello skupiny ochrany. Po instalaci služby hello spustí úlohu ochrany a selže. Po selhání hello budete potřebovat toomanually restartování každý počítač, který dosud byl hello nainstalovaná služba Mobility. Po restartování hello začne počítat od začátku hello úlohy ochrany a dojde k počáteční replikaci.
   >
   >

Můžete sledovat stav na hello **úlohy** stránky.

![Monitorování stavu na stránce úloh hello](./media/site-recovery-vmware-to-azure-classic/enable-protection5.png)

Stav ochrany je možné monitorovat také v **chráněné položky** > *název skupiny ochrany* > **virtuální počítače**. Po dokončení počáteční replikace a data se synchronizují, počítač změny stavu příliš**chráněné**.

![Stav monitorování v chráněné položky](./media/site-recovery-vmware-to-azure-classic/enable-protection6.png)

## <a name="step-11-set-protected-machine-properties"></a>Krok 11: Sada chráněných vlastnosti počítače.
1. Jakmile počítač **chráněné** stav, můžete konfigurovat její vlastnosti převzetí služeb při selhání. V hello podrobnosti skupiny ochrany, vyberte hello počítače a otevřete hello **konfigurace** kartě.
2. Site Recovery automaticky navrhuje vlastnosti hello virtuální počítač Azure a zjistí hello místní nastavení sítě.

    ![Nastavit vlastnosti virtuálního počítače](./media/site-recovery-vmware-to-azure-classic/vm-properties1.png)
3. Můžete změnit tato nastavení:

   * **Název virtuálního počítače Azure**: Toto je hello název, který bude mu udělená toohello počítače v Azure po převzetí služeb při selhání. Hello název musí splňovat požadavky pro Azure.
   * **Velikost virtuálního počítače Azure**: hello počet síťových adaptérů je závisí na velikosti hello je zadat hello cílového virtuálního počítače. Další informace o velikosti a adaptéry najdete v tématu hello [velikost tabulky](../virtual-machines/linux/sizes.md). Poznámky:

     * Když změníte hello velikost virtuálního počítače a uložit nastavení hello, hello počet síťových adaptérů se změní při otevření hello **konfigurace** kartě hello příště. Hello minimální počet síťových adaptérů cílových virtuálních počítačů je rovna toohello minimální počet síťových adaptérů na zdrojovém virtuálním počítači. maximální počet síťových adaptérů Hello je dáno hello velikost hello virtuálního počítače.
       * Pokud hello počet síťových adaptérů na zdrojovém počítači hello je menší než nebo rovna toohello počet adaptérů povoleno pro velikost cílového počítače hello, bude mít cíl hello hello stejný počet adaptérů jako zdroj hello.
       * Pokud hello počet adaptérů pro hello zdrojový virtuální počítač překračuje počet hello povolené pro velikost cílového hello, použije se maximální velikost cíle hello.
        Například pokud má zdrojový počítač dva síťové adaptéry a velikost hello cílového počítače podporuje čtyři, bude mít hello cílový počítač dva adaptéry. Pokud má hello zdrojový počítač dva adaptéry, ale velikost cílového hello podporované podporuje pouze jeden, bude mít cílový počítač hello jenom jeden adaptér.
     * Pokud hello virtuální počítač více síťových adaptérů, všechny adaptéry by měly být připojené toohello stejné síti Azure.
   * **Síť Azure**: je nutné zadat síť Azure, že se virtuální počítače Azure budou připojené tooafter převzetí služeb při selhání. Pokud nezadáte jeden, nebudou hello virtuálních počítačích Azure připojené tooany sítě. Kromě toho je nutné toospecify síť Azure, pokud chcete, aby toofailback z Azure toohello místní lokality. Navrácení služeb po obnovení vyžaduje propojení VPN mezi síť Azure a místní sítě.
   * **Azure podsíť IP adres nebo**: pro každý síťový adaptér vyberte hello podsíť toowhich hello by se měly připojit virtuální počítač Azure. Všimněte si, že pokud je síťový adaptér hello hello zdrojového počítače nakonfigurované toouse statickou IP adresu, je možné zadat statickou IP adresu pro hello virtuálního počítače Azure. Pokud nezadáte statickou IP adresu, bude přiděleno libovolná dostupná IP adresa. Pokud je zadaný hello cílová IP adresa, ale je již používán jiným virtuálním počítačem v Azure, převzetí služeb při selhání se nezdaří. Pokud je síťový adaptér hello hello zdrojového počítače nakonfigurované toouse DHCP, budete mít DHCP jako hello nastavení pro Azure.      

## <a name="step-12-create-a-recovery-plan-and-run-a-failover"></a>Krok 12: Vytvoření plánu obnovení a spusťte převzetí služeb při selhání
> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Enhanced-VMware-to-Azure-Failover/player]
>
>

Můžete spustit převzetí služeb při selhání pro jeden počítač, nebo můžete převzít více virtuálních počítačů, které provádějí hello stejné úlohy nebo spustit hello stejné zatížení. toofail přes více počítačů v hello stejný čas, přidáte tooa plánu obnovení.

toocreate plánu obnovení:

1. Na hello **plány obnovení** klikněte na tlačítko **plánu obnovení přidat** a přidejte plán obnovení. Zadejte podrobnosti pro plán hello a vyberte **Azure** jako cíl hello.

 ![Konfigurace plánu obnovení](./media/site-recovery-vmware-to-azure-classic/recovery-plan1.png)
2. V **vybrat virtuální počítač**, vyberte skupinu ochrany a pak vyberte počítače v plánu obnovení toohello tooadd skupiny hello.

 ![Přidat virtuální počítače](./media/site-recovery-vmware-to-azure-classic/recovery-plan2.png)

Můžete přizpůsobit hello plán toocreate skupin a pořadí hello pořadí, ve kterém jsou počítače v plánu obnovení hello při selhání. Můžete také přidat skripty a pokynů pro ruční akce. Skripty lze vytvořit ručně nebo pomocí [sad Azure Automation Runbook](site-recovery-runbook-automation.md). Další informace o přizpůsobení plánů obnovení najdete v tématu [vytvořit plány obnovení](site-recovery-create-recovery-plans.md).

## <a name="run-a-failover"></a>Spuštění převzetí služeb při selhání
Před spuštěním převzetí služeb při selhání:

* Ujistěte se, že tento server pro správu hello je spuštěné a dostupné. Pokud není, převzetí služeb při selhání se nezdaří.
* Pokud spustíte neplánované převzetí služeb při selhání:

  * Pokud je to možné, měli byste před spuštěním neplánovaného převzetí služeb při selhání vypnout primární počítače. Zajistíte tak, že nemáte obou hello zdroje a repliky počítačů spuštěných v hello stejnou dobu. Pokud replikujete virtuální počítače VMware, když spustíte neplánované převzetí služeb při selhání, můžete zadat, že Site Recovery opakovat tooshut dolů hello zdrojového počítače. V závislosti na stavu hello hello primární lokality tato operace může nebo nemusí fungovat. Pokud fyzické servery replikujete, není tato možnost nabízí Site Recovery.
  * Neplánované převzetí služeb při selhání se zastaví data replikace z primárních počítačů tak, aby žádná rozdílová data nebudou předávány po zahájení neplánovaného převzetí služeb při selhání.
  * Pokud chcete virtuální počítač repliky tooconnect toohello v Azure po převzetí služeb při selhání, povolte připojení ke vzdálené ploše na zdrojovém počítači hello před spuštěním převzetí služeb při selhání hello. Potom povolí připojení RDP přes bránu firewall hello. Musíte také tooallow protokolu RDP na hello veřejný koncový bod hello virtuálního počítače Azure po převzetí služeb při selhání. Postupujte podle [osvědčené postupy](http://social.technet.microsoft.com/wiki/contents/articles/31666.troubleshooting-remote-desktop-connection-after-failover-using-asr.aspx) tooensure, který RDP funguje po převzetí služeb při selhání.

> [!NOTE]
> tooget hello nejlepší výkon při tooAzure převzetí služeb při selhání, ujistěte se, nainstalovaný hello Azure Agent na počítači hello chráněný. To pomáhá spuštění počítače hello rychlejší a pomáhá diagnostikovat problémy. Hello Azure je k dispozici Agent pro [Linux](https://github.com/Azure/WALinuxAgent) a [Windows](http://go.microsoft.com/fwlink/?LinkID=394789).
>
>

### <a name="run-a-test-failover"></a>Spuštění testovacího převzetí služeb při selhání
Spusťte testovací převzetí služeb při selhání toosimulate vaše převzetí služeb při selhání a obnovení procesy v izolovanou síť, která nemá vliv provozním prostředí a umožňuje běžné replikace pokračovat normální. Testovací převzetí služeb při selhání je initiatd hello zdroje a můžete ho spustit v několika způsoby:

* **Nezadávejte síť Azure**: Pokud spustíte převzetí služeb při selhání bez připojení k síti, bude testovací hello zkontrolujte, zda virtuální počítače spustit a zobrazí správně v Azure. Virtuální počítače nebudou připojené tooan síť Azure po převzetí služeb při selhání.
* **Zadejte síť Azure**: Tento typ převzetí služeb při selhání zkontroluje, zda hello celé prostředí replikace se zobrazí podle očekávání a že jsou virtuální počítače Azure připojené toohello Zadaná síť.

toorun testovací převzetí služeb:

1. Na hello **plány obnovení** vyberte hello plán a klikněte na tlačítko **testovací převzetí služeb při selhání**.

 ![Vyberte plán hello](./media/site-recovery-vmware-to-azure-classic/test-failover1.png)
2. V **potvrďte testovací převzetí služeb při selhání**, vyberte **žádné** tooindicate nechcete toouse síť Azure hello testovací převzetí služeb při selhání, nebo vyberte hello sítě toowhich hello testovací virtuální počítače se připojí po převzetí služeb při selhání. Klikněte na tlačítko hello zaškrtnutí toostart hello převzetí služeb při selhání.

 ![Proveďte výběr](./media/site-recovery-vmware-to-azure-classic/test-failover2.png)
3. Sledovat průběh převzetí služeb při selhání na hello **úlohy** kartě.

 ![Monitorování průběhu](./media/site-recovery-vmware-to-azure-classic/test-failover3.png)
4. Po dokončení převzetí služeb při selhání hello, měli byste také mít možnost toosee hello repliky počítač Azure se zobrazí v **virtuální počítače** v hello portálu Azure. Pokud chcete tooinitiate toohello připojení RDP virtuálního počítače Azure, budete potřebovat tooopen portu 3389 na koncový bod virtuálního počítače hello.
5. Poté, co jste dokončení, když převzetí služeb při selhání dosáhne hello **Complete** testování fáze, klikněte na tlačítko **dokončení testovacího** toofinish. V **poznámky**, zaznamenejte a uložte jakékoli připomínky související s hello testovací převzetí služeb při selhání.
6. Klikněte na tlačítko **hello testovací převzetí služeb při selhání je dokončena** tooautomatically vyčištění hello testovací prostředí. Po dokončení se zobrazí hello testovací převzetí služeb při selhání **Complete** stavu. Všechny prvky nebo virtuální počítače automaticky vytvořené během hello testovací převzetí služeb při selhání se odstraní. Pokud převzetí služeb při selhání pokračovat déle než dva týdny, je nucen toofinish.

### <a name="run-an-unplanned-failover"></a>Spustit neplánované převzetí služeb při selhání
Neplánované převzetí služeb při selhání je zahájeno z Azure a možnost provádět i v případě hello primární lokalita není dostupná.

1. Na hello **plány obnovení** vyberte hello plán a klikněte na tlačítko **převzetí služeb při selhání** > **neplánované převzetí služeb při selhání**.

 ![Vyberte plán hello](./media/site-recovery-vmware-to-azure-classic/unplanned-failover1.png)
2. Pokud replikujete virtuální počítače VMware, můžete zkusit tooshut dolů místní virtuální počítače. Toto je akce typu best effort a převzetí služeb při selhání pokračovat, zda text hello úsilí úspěšné nebo ne. Pokud dojde k neúspěchu, podrobnosti o chybě se zobrazí na hello **úlohy** v části **neplánované převzetí služeb při selhání úlohy**.

 ![Možnost vypnutí místní virtuální počítače](./media/site-recovery-vmware-to-azure-classic/unplanned-failover2.png)

 > [!NOTE]
 > Tato možnost není dostupná, pokud replikujete fyzických serverů. Budete potřebovat tootry tooshut ty dolů ručně Pokud je to možné.
 >
 >

3. V **potvrzení převzetí služeb při selhání**zkontrolujte hello směr převzetí služeb při selhání (tooAzure) a vyberte bod obnovení hello chcete toouse hello převzetí služeb při selhání. Pokud jste povolili více virtuálních počítačů, když jste nakonfigurovali vlastnosti replikace, můžete obnovit bod toohello nejnovější aplikace nebo konzistentní při selhání obnovení. Můžete také vybrat **bod obnovení vlastní** toorecover tooan dříve bodu v čase. Klikněte na tlačítko hello zaškrtnutí toostart hello převzetí služeb při selhání.

 ![Potvrďte směr převzetí služeb při selhání](./media/site-recovery-vmware-to-azure-classic/unplanned-failover3.png)
4. Počkejte hello toofinish úlohy neplánované převzetí služeb při selhání. Můžete sledovat průběh převzetí služeb při selhání na hello **úlohy** kartě. I v případě, že během neplánované převzetí služeb při selhání dojít k chybám, plán obnovení hello běží, dokud se nedokončí. Měli byste také mít možnost toosee hello repliky počítač Azure se zobrazí v **virtuální počítače** v hello portálu Azure.

### <a name="connect-tooreplicated-azure-virtual-machines-after-failover"></a>Připojit tooreplicated Azure virtuální počítače po převzetí služeb při selhání
tooconnect tooreplicated virtuálních počítačů v Azure po převzetí služeb při selhání, budete potřebovat:

- Připojení vzdálené plochy na primární počítač hello povolena.
- Brána Windows Firewall v primárním počítači hello nastavení tooallow protokolu RDP.
- RDP přidat toohello veřejný koncový bod pro hello virtuální počítač Azure.

Další informace o toto nastavení najdete v tématu [řešení potíží s připojení ke vzdálené ploše po převzetí služeb při selhání pomocí automatické obnovení systému](http://social.technet.microsoft.com/wiki/contents/articles/31666.troubleshooting-remote-desktop-connection-after-failover-using-asr.aspx).

## <a name="deploy-additional-process-servers"></a>Nasadit další proces servery
Pokud musí horizontální škálování vašeho nasazení nad 200 zdrojového počítače, nebo pokud vaše celkové denní klidové vytížení větší než 2 TB, budete potřebovat další proces servery toohandle hello provoz svazku. tooset až další procesní server, zkontrolujte požadavky hello v [další proces servery](#additional-process-servers), a pak nastavte procesový server hello podle toohello následující pokyny. Po nastavení hello serveru, můžete nakonfigurovat toouse počítače zdrojového ho.

### <a name="set-up-an-additional-process-server"></a>Nastavení serveru další proces
Nastavení serveru další proces takto:

* Spusťte Průvodce tooconfigure hello unified serveru pro správu jako pouze procesní server.
* Pokud chcete replikaci dat toomanage pomocí pouze hello nový procesový server, musíte toomigrate chráněné počítače.

### <a name="install-hello-process-server"></a>Nainstalovat procesní server hello
1. Na hello **rychlý Start** stránky, stáhněte si hello sjednocený instalační soubor pro hello instalaci součásti Site Recovery. Spusťte instalační program.
2. V **před zahájením**, vyberte **přidat další proces servery tooscale se nasazení**.

 ![Přidání procesového serveru](./media/site-recovery-vmware-to-azure-classic/add-ps1.png)
3. Dokončete Průvodce hello stejně jako když jste [nastavit](#step-5-install-the-management-server) hello první server pro správu.
4. V **podrobnosti o konfiguraci serveru**, zadejte IP adresu hello hello původního serveru pro správu na kterém je nainstalovaný hello konfigurační server a pak zadejte přístupové heslo hello. Pokud nemáte heslo hello, spusťte  **<SiteRecoveryInstallationFolder>\home\sysystems\bin\genpassphrase.exe – n** na hello původní management server tooretrieve ho.

 ![Podrobnosti o konfiguraci serveru](./media/site-recovery-vmware-to-azure-classic/add-ps2.png)

### <a name="migrate-machines-toouse-hello-new-process-server"></a>Migrace počítačů toouse hello nový procesový server
1. Otevřete **konfigurační servery** > **Server** > *názvem původního serveru pro správu hello*  >   **Podrobnosti o serveru**.

 ![Podrobnosti o serveru](./media/site-recovery-vmware-to-azure-classic/update-process-server1.png)
2. V hello **proces servery** seznamu, vyberte **změnit procesový Server** další toohello serveru, které chcete toochange.

 ![Aktualizace hello procesového serveru](./media/site-recovery-vmware-to-azure-classic/update-process-server2.png)
3. Vyberte **změnit procesový Server**, vyberte **cílový procesový Server**, a pak vyberte hello nový server pro správu. Pak bude zpracovávat vyberte hello virtuálních počítačů, které hello nový procesový server. Klikněte na tlačítko hello informace ikonu tooget informace o serveru hello. Hello průměrné místo, které je potřeba tooreplicate každý vybraný virtuální počítač toohello nový procesový server je zobrazené toohelp provedené načíst rozhodnutí. Klikněte na tlačítko toostart zaškrtnutí hello replikace toohello nový procesový server.

 ![Změnit hello procesový server](./media/site-recovery-vmware-to-azure-classic/update-process-server3.png)

## <a name="vmware-permissions-for-vcenter-access"></a>VMware oprávnění pro přístup k systému vCenter
procesový server Hello může automaticky zjistit virtuální počítače na vCenter server. tooperform automatického zjišťování, musíte toodefine roli (Azure_Site_Recovery) v systému vCenter server hello Site Recovery tooaccess hello vCenter úrovně tooallow. Pokud potřebujete tooAzure počítače VMware toomigrate pouze a nepotřebujete toofailback z Azure, můžete definovat roli jen pro čtení, která je dostačující. Nastavení oprávnění hello, jak je popsáno v [krok 6: nastavení pověření pro hello vCenter server](#step-6-set-up-credentials-for-the-vcenter-server). oprávnění role Hello jsou shrnuty v následující tabulce hello:

| **Role** | **Podrobnosti** | **Oprávnění** |
| --- | --- | --- |
| Azure_Site_Recovery role |Virtuální počítač VMware zjišťování |Přiřadíte tato oprávnění pro server hello v Center:<br/><br/>Úložiště dat: Přidělit prostor, procházet úložiště, operace se soubory nízké úrovně, odstraňte soubor, soubory aktualizace virtuálního počítače<br/><br/>Síť: Přiřadit sítě<br/><br/>Prostředek: Přiřadit tooresource fondu virtuálních počítačů, migrací používá technologii vypnout virtuální počítač, migrace používá technologii na virtuální počítač<br/><br/>Úlohy: Vytvoření úlohy, úloha aktualizace<br/><br/>Virtuální počítač > Konfigurace<br/><br/>Virtuální počítač > interakcí > odpovědět na otázku, připojení zařízení, nakonfigurovat CD média, konfigurovat disketová média, vypnutí napájení, instalaci nástroje VMware<br/><br/>Virtuální počítač > inventáře > vytvořit, registrace, Unregister<br/><br/>Virtuální počítač > zřizování > Povolit stahování virtuálního počítače, nahrávání souborů virtuálního počítače povolit<br/><br/>Virtuální počítač > snímky > odebrat snímky |
| role uživatele vCenter |Virtuální počítač VMware zjišťování nebo převzetí služeb při selhání bez vypnutí zdrojového virtuálního počítače |Přiřadíte tato oprávnění pro server hello v Center:<br/><br/>Objekt datového centra > Propagate tooChild objektu, role = jen pro čtení <br/><br/>uživatel Hello je přiřazena na úrovni hello datacenter a má přístup k objektům hello tooall tedy hello datacenter. Pokud chcete přístup hello toorestrict, přiřaďte hello **žádný přístup** role s hello **rozšířit toochild** objektu toohello podřízené objekty (hostitelů ESX, datastores, virtuální počítače a sítě). |
| role uživatele vCenter |Převzetí služeb při selhání a navrácení služeb po obnovení |Přiřadíte tato oprávnění pro server hello v Center:<br/><br/>Objekt Datacenter – Propagate toochild objektu, role = Azure_Site_Recovery<br/><br/>uživatel Hello je přiřazena na úrovni datacenter a má přístup k objektům hello tooall tedy hello datacenter.  Pokud chcete přístup hello toorestrict, přiřaďte hello ** žádný přístup ** role s hello **Propagate toochild objekt** toohello podřízený objekt (hostitelů ESX, datastores, virtuální počítače a sítě). |

## <a name="third-party-software-notices-and-information"></a>Oznámení software třetích stran a informace
<!--Do Not Translate or Localize-->

Hello software a firmware spuštěné v hello produktů společnosti Microsoft nebo služba je založena na nebo zahrnuje materiálu z hello projekty uvedené níže (dále souhrnně nazývané "kód třetí strany").  Společnost Microsoft se hello není původní autor hello kód třetích stran.  Hello původní autorská práva a licenci, pod kterým společnost Microsoft takový kód třetích stran, jsou nastavené níže uvedenými.

Hello informace v části A týká níže uvedené součásti z projektů hello kód třetích stran. Tyto licence a informace jsou uvedené pro pouze pro informační účely.  Tento kód třetích stran, která má být relicensed tooyou společností Microsoft v rámci licenční podmínky pro produkty Microsoft hello nebo služby společnosti Microsoft software.  

Hello informace v části B je týkající se součástí kód třetích stran, které jsou určené k dispozici tooyou společností Microsoft v rámci hello původní licenční podmínky.

Hello celého souboru může být nalezen na hello [Microsoft Download Center](http://go.microsoft.com/fwlink/?LinkId=529428). Společnost Microsoft si vyhrazuje všechna práva, která nejsou výslovně udělena, zda implicitně, jako překážku uplatnění nároku či jiným způsobem.

## <a name="next-steps"></a>Další kroky
[Další informace o navrácení služeb po obnovení](site-recovery-failback-azure-to-vmware-classic.md) toobring vaše počítače při selhání spuštění v Azure zpět tooyour v místním prostředí.
